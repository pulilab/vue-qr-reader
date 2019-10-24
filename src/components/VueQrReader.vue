<template>
  <div class="vue-qr-reader__container" ref="container">
    <video
      v-show="showPlay"
      class="source"
      ref="video"
      :width="videoWH.width"
      :height="videoWH.height"
      controls
    ></video>
    <canvas v-show="!showPlay" ref="canvas"></canvas>
    <button v-show="showPlay" @click="run">Play!</button>
  </div>
</template>

<script>
import jsQR from "jsqr";

export default {
  name: "vue-qr-reader",
  props: {
    useBackCamera: {
      type: Boolean,
      default: true
    },
    stopOnScanned: {
      type: Boolean,
      default: true
    },
    drawOnFound: {
      type: Boolean,
      default: true
    },
    lineColor: {
      type: String,
      default: "#FF3B58"
    },
    lineWidth: {
      type: Number,
      default: 2
    },
    videoWidth: {
      type: Number,
      default: 320
    },
    videoHeight: {
      type: Number,
      default: 240
    },
    responsive: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      showPlay: false,
      containerWidth: null,
      active: false
    };
  },
  computed: {
    videoWH() {
      if (this.containerWidth) {
        const width = this.containerWidth;
        const height = width * 0.75;
        return { width, height };
      }
      return { width: this.videoWidth, height: this.videoHeight };
    }
  },
  methods: {
    drawLine(begin, end) {
      this.canvas.beginPath();
      this.canvas.moveTo(begin.x, begin.y);
      this.canvas.lineTo(end.x, end.y);
      this.canvas.lineWidth = this.lineWidth;
      this.canvas.strokeStyle = this.lineColor;
      this.canvas.stroke();
    },
    drawBox(l) {
      if (this.drawOnFound) {
        this.drawLine(l.topLeftCorner, l.topRightCorner);
        this.drawLine(l.topRightCorner, l.bottomRightCorner);
        this.drawLine(l.bottomRightCorner, l.bottomLeftCorner);
        this.drawLine(l.bottomLeftCorner, l.topLeftCorner);
      }
    },
    tick() {
      if (
        this.$refs.video &&
        this.$refs.video.readyState === this.$refs.video.HAVE_ENOUGH_DATA
      ) {
        this.$refs.canvas.height = this.videoWH.height;
        this.$refs.canvas.width = this.videoWH.width;
        this.canvas.drawImage(
          this.$refs.video,
          0,
          0,
          this.$refs.canvas.width,
          this.$refs.canvas.height
        );
        const imageData = this.canvas.getImageData(
          0,
          0,
          this.$refs.canvas.width,
          this.$refs.canvas.height
        );
        let code = false;
        try {
          code = jsQR(imageData.data, imageData.width, imageData.height);
        } catch (e) {
          // sometimes JSQR may fail, but we can carry on.
          console.error(e);
        }
        if (code) {
          this.drawBox(code.location);
          this.found(code.data);
        }
      }
      this.run();
    },
    setup() {
      if (this.responsive) {
        this.$nextTick(() => {
          this.containerWidth = this.$refs.container.clientWidth;
        });
      }
      if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
        this.previousCode = null;
        this.parity = 0;
        this.active = true;
        this.canvas = this.$refs.canvas.getContext("2d");
        const facingMode = this.useBackCamera
          ? { exact: "environment" }
          : "user";
        const handleSuccess = stream => {
          if (this.$refs.video.srcObject !== undefined) {
            this.$refs.video.srcObject = stream;
          } else if (videoEl.mozSrcObject !== undefined) {
            this.$refs.video.mozSrcObject = stream;
          } else if (window.URL.createObjectURL) {
            this.$refs.video.src = window.URL.createObjectURL(stream);
          } else if (window.webkitURL) {
            this.$refs.video.src = window.webkitURL.createObjectURL(stream);
          } else {
            this.$refs.video.src = stream;
          }
          // iOS play inline
          this.$refs.video.playsInline = true;
          const playPromise = this.$refs.video.play();
          playPromise.catch(() => (this.showPlay = true));
          playPromise.then(this.run);
        };
        navigator.mediaDevices
          .getUserMedia({ video: { facingMode } })
          .then(handleSuccess)
          .catch(() => {
            navigator.mediaDevices
              .getUserMedia({ video: true })
              .then(handleSuccess)
              .catch(error => {
                this.$emit("error-captured", error);
              });
          });
      }
    },
    run() {
      if (this.active) {
        requestAnimationFrame(this.tick);
      }
    },
    found(code) {
      if (this.previousCode !== code) {
        this.previousCode = code;
      } else if (this.previousCode === code) {
        this.parity += 1;
      }
      if (this.parity > 2) {
        this.active = this.stopOnScanned ? false : true;
        this.parity = 0;
        this.$emit("code-scanned", code);
      }
    },
    fullStop() {
      if (this.$refs.video && this.$refs.video.srcObject) {
        this.$refs.video.srcObject.getTracks().forEach(t => t.stop());
      }
    }
  },
  mounted() {
    this.setup();
  },
  beforeDestroy() {
    this.fullStop();
  },
  watch: {
    active: {
      immediate: true,
      handler(active) {
        if (!active) {
          this.fullStop();
        }
      }
    }
  }
};
</script>

<style scoped>
.vue-qr-reader__container {
  width: 100%;
  height: auto;
}
</style>
