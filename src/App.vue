<template>
  <div id="app">
    <vue-qr-reader  v-on:code-scanned="codeScanned" :stop-on-scanned="false" :draw-on-found="true" :responsive="false"/>
    {{scanned}}
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
      scanned: ""
    };
  },
  methods: {
    codeScanned(code) {
      this.scanned = code;
    }
  }
};
</script>

<style>

</style>
