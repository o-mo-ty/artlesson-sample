<template>
  <div id="app" @scroll="scrollTest">
    <SalesOrderSelect/>
    <SalesMetricSelect/>
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div style="height: 1500px"></div>
    <div v-if="showDum" style="height: 300px"></div>
    <button @click="onShow">showDum</button>
    <Chart :scrollY="scrollY"/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Chart from "./components/Chart.vue";
import SalesOrderSelect from "./components/SalesOrderSelect.vue";
import SalesMetricSelect from "./components/SalesMetricSelect.vue";

export default {
  name: "App",
  components: {
    // HelloWorld,
    SalesOrderSelect,
    SalesMetricSelect,
    Chart,
  },
  data() {
    return {
      scrollY: 0,
      showDum: false
    };
  },
  mounted() {
    window.addEventListener("scroll", this.scrollTest);
  },
  methods: {
    onShow() {
      this.showDum =  true;
    },
    scrollTest() {
      console.log("scrolling");
      this.scrollY = window.scrollY
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
