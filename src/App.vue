<template>
  <div id="app">
    <VueQRReader  v-on:code-scanned="codeScanned" :stop-on-scanned="false" :draw-on-found="true" :responsive="false"/>
    {{scanned}}
  </div>
</template>

<script>
import VueQRReader from "./components/VueQRReader.vue";

export default {
  name: "app",
  components: {
    VueQRReader
  },
  data () {
    return {
      scanned: ''
    };
  },
  methods: {
    codeScanned (code) {
      this.scanned = code;
    }
  }
};</script>

<style>
</style>
