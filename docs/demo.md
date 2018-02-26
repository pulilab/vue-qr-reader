# Demo


>This is an interactive demo, please accept the Webcam permission pop-up in the browser.


<vuep template="#base"></vuep>

<script v-pre type="text/x-template" id="base">
  <template>
    <div>
      <div>Value: {{scanned}}</div>
      <vue-qr-reader ref="qr"
        v-on:code-scanned="codeScanned"
        stop-on-scanned="true"
        use-back-camera="true"
        draw-on-found="true"
        line-color="#00FF00"
        line-width="20"
        video-height="480"
        video-width="640"
        responsive="true">
      </vue-qr-reader>
    </div>
  </template>

  <script>
    module.exports = {
      data: function () {
        return {
          scanned: ''
        }
      },
      methods: {
        codeScanned (event) {
          this.scanned = event.detail[0];
        }
      }
    }
  </script>
</script>
