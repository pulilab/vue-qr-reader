<template>
  <div id="app">
    <vue-qr-reader
      v-if="show"
      v-on:code-scanned="codeScanned"
      v-on:error-captured="errorCaptured"
      :stop-on-scanned="true"
      :draw-on-found="true"
      :responsive="false"
    />
    {{ scanned }}
    <button @click="show = !show">Toggle Video</button>
  </div>
</template>

<script>
import VueQrReader from "./components/VueQrReader.vue";

export default {
  name: "app",
  components: {
    VueQrReader
  },
  data() {
    return {
      errorMessage: "",
      scanned: "",
      show: true
    };
  },
  methods: {
    codeScanned(code) {
      this.scanned = code;
    },
    errorCaptured(error) {
      switch (error.name) {
        case "NotAllowedError":
          this.errorMessage = "Camera permission denied.";
          break;
        case "NotFoundError":
          this.errorMessage = "There is no connected camera.";
          break;
        case "NotSupportedError":
          this.errorMessage =
            "Seems like this page is served in non-secure context.";
          break;
        case "NotReadableError":
          this.errorMessage =
            "Couldn't access your camera. Is it already in use?";
          break;
        case "OverconstrainedError":
          this.errorMessage = "Constraints don't match any installed camera.";
          break;
        default:
          this.errorMessage = "UNKNOWN ERROR: " + error.message;
      }
      console.error(this.errorMessage);
    }
  }
};
</script>

<style></style>
