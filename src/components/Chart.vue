<template>
  <div id="chart">
    <div
      v-show="showToolTip"
      class="tooltip-condition"
      :style="{
        left: `${toolTipLeftPosition}px`,
        width: `${toolTipWidth}px`,
        top: `${toolTipTopPosition}px`,
      }"
    >
      <div style="padding-top: 10px; padding-left: 32px">
        <span class="tooltip-data">{{ toolTipDisplayYear }}</span
        ><span class="tooltip-unit">年</span
        ><span class="tooltip-data">{{ toolTipDisplayMonth }}</span
        ><span class="tooltip-unit">月</span>
      </div>

      <div style="padding-top: 10px; padding-left: 10px">
        <span class="tooltip-unit">売上</span
        ><span class="tooltip-data">{{ toolTipDisplaySales }}</span
        ><span class="tooltip-unit">千円</span>
      </div>
      <div style="padding-top: 10px; padding-left: 10px">
        <span class="tooltip-unit">回収率</span
        ><span class="tooltip-data">{{ toolTipDisplayCollectionRate }}</span
        ><span class="tooltip-unit">%</span>
      </div>
    </div>
    <div
      class="tooltip-bar"
      v-show="showToolTip"
      :style="{
        left: `${toolTipBarPosition}px`,
        top: `${toolTipTopPosition}px`,
      }"
    />
    <apexchart
      type="line"
      height="350"
      :width="chartWidth"
      :options="chartOptions"
      :series="series"
      @dataPointMouseEnter="onDataPointMouseEnter"
      @dataPointMouseLeave="onDataPointMouseLeave"
      @mounted="onMounted"
      @updated="onUpdated"
    ></apexchart>
    <!-- yaxisの幅 -->
    <div class="testbar" :style="{ width: yaxisRight + 'px' }"></div>
    <!-- chartまでの幅 -->
    <div class="testbar" :style="{ width: chartLeft + 'px' }"></div>
    <!-- <div style="display: flex"> -->
    <!-- 表：行ヘッダの終わり位置 -->
    <div
      class="table"
      :style="{
        width: rowColumnWidth + 'px',
        border: '1px solid #707070',
        'margin-top': '10px',
      }"
    >
      行ヘッダ
    </div>
    <div
      class="table"
      :style="{
        width:
          rowColumnWidth +
          columnWidth * 1 -
          barwidth / 2 -
          barSpacing / 2 -
          1 +
          'px',
        border: '1px solid #707070',
        'margin-top': '10px',
      }"
    >
      データ
    </div>
    <div
      class="table"
      :style="{
        width:
          rowColumnWidth +
          columnWidth * 2 -
          barwidth / 2 -
          barSpacing / 2 -
          1 +
          'px',
        border: '1px solid #707070',
        'margin-top': '10px',
      }"
    >
      データ
    </div>
    <div
      class="table"
      :style="{
        width:
          rowColumnWidth +
          columnWidth * 3 -
          barwidth / 2 -
          barSpacing / 2 -
          1 +
          'px',
        border: '1px solid #707070',
        'margin-top': '10px',
      }"
    >
      データ
    </div>
    <div
      class="table"
      :style="{
        width:
          rowColumnWidth +
          columnWidth * 4 -
          barwidth / 2 -
          barSpacing / 2 -
          1 +
          'px',
        border: '1px solid #707070',
        'margin-top': '10px',
      }"
    >
      データ
    </div>
    <!-- データ行:1 -->
    <!-- <div
        v-for="n of 4"
        :key="n"
        class="table"
        :style="{
          width: columnWidth / 2 + 'px',
          border: 'solid #707070',
          'border-width': '1px 1px 1px 0px',
          'margin-top': '10px',
        }"
      ></div> -->
    <!-- </div> -->
    <div
      class="testbar"
      :style="{
        width: chartLeft - barSpacing / 2 + firstBarLeftPadding + 'px',
      }"
    ></div>
    <div
      class="testbar"
      :style="{
        width:
          chartLeft -
          barSpacing / 2 +
          firstBarLeftPadding +
          barSpacing +
          barwidth +
          'px',
      }"
    ></div>
    <div
      class="underbar"
      :style="{
        position: 'absolute',
        top: chartXaxisLinePosition + 'px',
        left: chartEventXPosition - barSpacing / 2 + 'px',
        width: barwidth + barSpacing + 'px',
      }"
    ></div>
    <div>{{ xxx }}</div>
    <button @click="onRead">Read!!!!!!!!!!</button>
  </div>
</template>

<script>
// ■Mixed Chart Sample
// https://apexcharts.com/vue-chart-demos/mixed-charts/line-column/
// ■Options Reference
// https://apexcharts.com/docs/installation/#
// ■tooltip Doc
// https://apexcharts.com/docs/options/tooltip/

const SALES_DATA_INDEX = 0;
const COLLECTION_RATE_DATA_INDEX = 1;

export default {
  name: "Chart",
  props: {
    scrollY: 0,
  },
  data() {
    return {
      events: [],
      currentEvent: null,
      currentConfig: null,
      chartWidth: 1000,
      toolTipWidth: 400,
      showToolTip: false,
      barwidth: null,
      yaxisRight: null,
      chartLeft: null,
      barSpacing: null,
      firstBarLeftPadding: null,
      rowColumnWidth: null, // ヘッダ行の幅
      columnWidth: null, // データ行の幅
      chartXaxisLinePosition: null,
      series: [
        {
          name: "sales",
          type: "column",
          data: [
            440111, 505111, 414111, 671111, 227111, 413111, 201111, 3521111,
            752111, 320111, 257111, 160111,
          ],
        },
        {
          name: "collectionRate",
          type: "line",
          data: [23, 42, 35, 27, 43, 22, 17, 31, 22, 22, 12, 16],
        },
      ],
      chartOptions: {
        plotOptions: {
          bar: {
            columnWidth: "30%",
          },
        },
        legend: {
          show: false,
        },
        // https://apexcharts.com/docs/options/states/
        states: {
          hover: {
            filter: {
              type: "none",
            },
          },
        },
        chart: {
          // https://apexcharts.com/docs/options/chart/toolbar/
          toolbar: {
            show: false,
          },
          // https://apexcharts.com/docs/options/chart/zoom/
          zoom: {
            enabled: false,
          },
          height: 350,
          type: "line",
          background: "#4D4E64", // チャート自体の背景色
          foreColor: "#FFFFFF", // チャート自体の文字色
        },
        fill: {
          type: "gradient",
          gradient: {
            type: "vertical",
            shadeIntensity: 1,
            gradientToColors: ["#55BBDD", "#FFFFFF"],
            opacityTo: 0.9,
            stops: [70, 100],
          },
        },
        colors: ["#FFFFFF"], // ラインチャートのラインの色
        markers: {
          size: 8,
          colors: "#44BBFF", // ラインチャートのポイントの色
        },
        tooltip: {
          enabled: false,
        },
        stroke: {
          width: [0, 4],
        },
        // https://apexcharts.com/docs/options/title/
        title: {
          text: "売上",
          align: "left",
          style: {
            fontSize: "18px",
            fontWeight: "bold",
          },
        },
        subtitle: {
          text: "単位：（千円）",
          align: "left",
          offsetX: -40,
        },
        // https://apexcharts.com/docs/options/xaxis/
        xaxis: {
          axisBorder: {
            color: "#686977",
          },
          type: "category",
          categories: [
            "201001",
            "202002",
            "203003",
            "204004",
            "205005",
            "206006",
            "207007",
            "208008",
            "209009",
            "210010",
            "211011",
            "212012",
          ],
          labels: {
            formatter: function (value) {
              // console.log("formatterValue", value);
              if (!value) return [];
              return [value.slice(0, 4), Number(value.slice(4, 6)).toString()];
            },
            style: {
              fontSize: "18px",
              fontWeight: 400,
              fontFamily: "Roboto",
              cssClass: "apexcharts-xaxis-label",
            },
          },
          tooltip: {
            formatter: undefined,
            offsetY: -280,
            style: {
              fontSize: 0,
              fontFamily: 0,
            },
          },
          tickPlacement: "on",
        },
        yaxis: [
          {
            // floating: true,
            labels: {
              offsetX: -60,
              align: "right",
              minWidth: 300,
              // maxWidth: 160,
              style: {
                fontSize: "16px",
                fontFamily: "Roboto",
                fontWeight: 600,
                cssClass: "apexcharts-yaxis-title",
              },
            },
            // axisBorder: {
            //   show: true,
            //   color: "#78909C",
            //   offsetX: -10,
            //   offsetY: 0,
            // },
          },
          {
            opposite: true,
            labels: {
              offsetX: 10,
              style: {
                fontSize: "16px",
                fontFamily: "Roboto",
                fontWeight: 600,
                cssClass: "apexcharts-yaxis-title",
              },
              padding: {
                right: 0,
              },
            },
          },
        ],
        grid: {
          padding: {
            left: 0,
          },
        },
      },
    };
  },
  watch: {
    // 問題内容が変更されるたびに、関数が実行されます。
    scrollY(newVal, oldVal) {
      // console.log('WATCH!!!!')
      // this.readPosition();
    }
  },
  computed: {
    xxx() {
      const x = [];
      this.events.forEach((event, index) => {
        if (index === 0) {
          x.push(event.target.getBoundingClientRect().left);
          return;
        }
        const lastX =
          this.events[index - 1].target.getBoundingClientRect().left;
        console.log("left", event.target.getBoundingClientRect().left);
        x.push(event.target.getBoundingClientRect().left - lastX);
      });
      // const x = [];
      // // return this.events.map((event) => event.target.getBoundingClientRect().left)
      // for (let i = 0; i < this.events.length - 1; i++) {
      //   if (i === 0) return;
      //   const yyy =
      //     this.events[i - 1].event.target.getBoundingClientRect().left;
      //   const zzz = this.events[i].event.target.getBoundingClientRect().left;
      //   x.push(zzz - yyy);
      // }
      // console.log("xxxxx", x);
      return x;
    },
    toolTipLeftPosition() {
      // バーのViewPort基準のx - ツールチップの横幅/2 + バーの幅/2-
      return (
        this.chartEventXPosition -
        this.toolTipWidth / 2 +
        this.chartBarWidth / 2
      );
    },
    toolTipTopPosition() {
      // バーのViewPort基準のx - ツールチップの横幅/2 + バーの幅/2-
      return this.chartEventYPosition;
    },
    toolTipBarPosition() {
      return this.chartEventXPosition + this.chartBarWidth / 2;
    },
    chartBarWidth() {
      if (this.currentEvent === null) return 0;
      return (
        this.currentEvent.target.getBoundingClientRect().right -
        this.currentEvent.target.getBoundingClientRect().left
      );
    },
    chartEventXPosition() {
      if (this.currentEvent === null) return 0;
      const xOffSet = window.pageXOffset;
      return this.currentEvent.target.getBoundingClientRect().x + xOffSet;
    },
    toolTipDisplayYear() {
      if (this.currentConfig === null) return "";
      const selectionIndex_x = this.currentConfig.dataPointIndex;
      return this.chartOptions.xaxis.categories[selectionIndex_x].slice(0, 4);
    },
    toolTipDisplayMonth() {
      if (this.currentConfig === null) return "";
      const selectionIndex_x = this.currentConfig.dataPointIndex;
      return this.chartOptions.xaxis.categories[selectionIndex_x].slice(4, 6);
    },
    toolTipDisplaySales() {
      if (this.currentConfig === null) return "";
      const selectionIndex_x = this.currentConfig.dataPointIndex;
      return this.series[SALES_DATA_INDEX].data[selectionIndex_x];
    },
    toolTipDisplayCollectionRate() {
      if (this.currentConfig === null) return "";
      const selectionIndex_x = this.currentConfig.dataPointIndex;
      return this.series[COLLECTION_RATE_DATA_INDEX].data[selectionIndex_x];
    },
  },
  methods: {
    decorateTitle() {
      const mainTitle = document.getElementsByClassName(
        "apexcharts-title-text"
      )[0];
      const subTitle = document.getElementsByClassName(
        "apexcharts-subtitle-text"
      )[0];

      const copyMainTitle = mainTitle.cloneNode(false);
      const copySubTitle = subTitle.cloneNode(false);

      copyMainTitle.textContent = "回収率";
      copySubTitle.textContent = "単位：（％）";

      copyMainTitle.setAttribute("x", this.chartWidth - 40);
      copySubTitle.setAttribute("x", this.chartWidth - 40);

      copyMainTitle.removeAttribute("id");
      copySubTitle.removeAttribute("id");

      mainTitle.after(copyMainTitle);
      subTitle.after(copySubTitle);
    },
    decorateUnit() {
      const svgElements = document.querySelectorAll(".apexcharts-xaxis-label");

      svgElements.forEach((svg) => {
        const tspans = svg.getElementsByTagName("tspan");
        const yearValue = tspans[0];
        const monthValue = tspans[1];

        yearValue.insertAdjacentHTML(
          "afterend",
          '<tspan class="year" font-size="15px" font-style="normal">年</tspan>'
        );
        monthValue.insertAdjacentHTML(
          "afterend",
          '<tspan class="month" font-size="15px" font-style="normal">月</tspan>'
        );
      });
    },
    onDataPointMouseEnter: function (event, chartContext, config) {
      console.log("mouseEnter");
      this.events.push(event);
      this.currentEvent = event;
      this.currentConfig = config;
      // 折れ線グラフの場合、ツールチップを表示しない
      if (config.seriesIndex === 1) return;
      this.showToolTip = true;
    },
    onDataPointMouseLeave() {
      this.currentEvent = null;
      this.currentConfig = null;
      this.showToolTip = false;
    },
    onMounted() {
      console.log("mounted");
      this.decorateUnit();
      this.decorateTitle();
      this.readPosition();
    },
    onUpdated() {
      console.log("updated");
      this.decorateUnit();
      this.decorateTitle();
    },
    onRead() {
      this.readPosition();
    },
    readPosition() {
      const yOffSet = window.pageYOffset;
      const chartMarginLeft = document
        .getElementById("chart")
        .getBoundingClientRect().left;
      this.yaxisRight =
        document
          .getElementsByClassName("apexcharts-yaxis")[0]
          .getBoundingClientRect().right - chartMarginLeft;
      this.chartLeft =
        document
          .getElementsByClassName("apexcharts-grid")[0]
          .getBoundingClientRect().left - chartMarginLeft;
      this.chartXaxisLinePosition = document
        .getElementsByClassName("apexcharts-grid")[0]
        .getBoundingClientRect().bottom + yOffSet;
      const bar1Left =
        document
          .getElementsByClassName("apexcharts-bar-area")[0]
          .getBoundingClientRect().left - chartMarginLeft;
      const bar1Right =
        document
          .getElementsByClassName("apexcharts-bar-area")[0]
          .getBoundingClientRect().right - chartMarginLeft;
      const bar2Left =
        document
          .getElementsByClassName("apexcharts-bar-area")[1]
          .getBoundingClientRect().left - chartMarginLeft;
      this.barwidth = bar1Right - bar1Left;
      this.firstBarLeftPadding = bar1Left - this.chartLeft;
      this.barSpacing = bar2Left - bar1Right;
      this.rowColumnWidth =
        this.chartLeft - this.barSpacing / 2 + this.firstBarLeftPadding;
      this.columnWidth = this.barwidth + this.barSpacing;

      console.log("chartXaxisLinePosition", this.chartXaxisLinePosition);
      // console.log("barwidth", this.barwidth);
      // console.log("firstBarLeftPadding", this.firstBarLeftPadding);
      // console.log("chartMarginLeft", chartMarginLeft);
      // console.log("yaxisRight", this.yaxisRight);
      // console.log("chartLeft", this.chartLeft);
      // console.log("bar1Right", bar1Right);
      // console.log("bar2Left", bar2Left);
      // console.log("barSpacing", this.barSpacing);
    },
  },
};
</script>

<style scoped>
.underbar {
  border-top: 1px solid #ee6688;
  border-left: 1px solid #ee6688;
  border-right: 1px solid #ee6688;
  height: 1200px;
  /* background-color: #ee6688; */
  z-index: 1000;
}
.testbar {
  height: 2px;
  background-color: red;
  margin-top: 10px;
}
/* .testborder {
  height: 2px;
  border:#ee6688;
  margin-top: 10px;
}  */
.hide {
  display: none;
}
#chart >>> .tooltip-unit {
  font-size: 14px;
  display: inline-block;
  padding: 0 2px;
}
#chart >>> .tooltip-data {
  font-size: 22px;
  font-weight: bold;
  font-style: italic;
}
#chart >>> .tooltip-condition {
  background: #ee6688;
  display: flex;
  position: absolute;
  border-radius: 30px;
  height: 50px;
  z-index: 100;
  border: 5px solid white;
  color: white;
}
.tooltip-bar {
  background: #ee6688;
  position: absolute;
  height: 880px;
  width: 1px;
  z-index: 5;
}
#chart >>> .apexcharts-bar-area:hover {
  fill: #ee6688;
}

#chart >>> .apexcharts-yaxis-title {
  font-style: italic;
}
#chart >>> .apexcharts-xaxis-label {
  font-style: italic;
}

.apexcharts-grid {
}
</style>