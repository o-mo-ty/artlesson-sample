<template>
  <div id="app" @scroll="scrollTest">
    <h3>連動するやつ</h3>
    <SalesOrderSelect/>
    <div class="spacer"></div>
    <h3>マルチセレクト(具体値込みのやつ)</h3>
    <SalesMetricSelect/>
    <div class="spacer"></div>
    <h3>階層持ちのやつ（これから作る）</h3>
    <div class="spacer"></div>
    <div style="height: 1500px"></div>
    <div v-if="showDum" style="height: 300px"></div>
    <button @click="onShow">showDum</button>
    <Chart :scrollY="scrollY"/>
  </div>
</template>

<script>
import Chart from "./components/Chart.vue";
import SalesOrderSelect from "./components/SalesOrderSelect.vue";
import SalesMetricSelect from "./components/SalesMetricSelect.vue";

export default {
  name: "App",
  components: {
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
.spacer {
  height: 0px;
  border: 1px solid #ccc;
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>
