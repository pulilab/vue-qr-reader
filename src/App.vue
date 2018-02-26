<template>
  <div id="app">
    <vue-qr-reader v-if="show"  v-on:code-scanned="codeScanned" :stop-on-scanned="true" :draw-on-found="true" :responsive="false"/>
    {{scanned}}
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
      scanned: "",
      show: true
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
