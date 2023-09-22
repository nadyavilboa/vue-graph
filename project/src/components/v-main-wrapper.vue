<template>
    <div class="v-main-wrapper">
        <v-graph
            :max-value="maxValue"
            :value="value"
            :targetValue="targetValue"
        />
    </div>
  </template>
  
  <script>
  import vGraph from './v-graph.vue';
  export default {
    name: 'v-main-wrapper',
    components: {
        vGraph
    },
    data() {
        return {
            maxValue: 108,
            value: 35,
            targetValue: 80
        }
    },
  }
  </script>
  
  <style lang="scss" scoped>
    *, html, body {
        margin: 0;
        padding: 0;
        font-family: 'Poppins', sans-serif;
        box-sizing: border-box;
    }

    html, body, #app {
        width: 100%;
        height: 100%;
    }

    .v-main-wrapper {
        background: #2e3640;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
  </style>
  