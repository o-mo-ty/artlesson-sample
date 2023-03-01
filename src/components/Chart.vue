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
      :style="{ left: `${toolTipBarPosition}px`, top: `${toolTipTopPosition}px` }"
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
  data() {
    return {
      currentEvent: null,
      currentConfig: null,
      chartWidth: 1600,
      toolTipWidth: 400,
      showToolTip: false,
      series: [
        {
          name: "sales",
          type: "column",
          data: [440, 505, 414, 671, 227, 413, 201, 352, 752, 320, 257, 160],
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
            color:'#686977' ,
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
              console.log("formatterValue", value);
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
            labels: {
              offsetX: 20,
              style: {
                fontSize: "16px",
                fontFamily: "Roboto",
                fontWeight: 600,
                cssClass: "apexcharts-yaxis-title",
              },
            },
          },
          {
            opposite: true,

            labels: {
              style: {
                fontSize: "16px",
                fontFamily: "Roboto",
                fontWeight: 600,
                cssClass: "apexcharts-yaxis-title",
              },
              padding: {
                right: 10,
              },
            },
          },
        ],
        grid: {
  padding: {
    left: 30
  }
},
      },
    };
  },
  computed: {
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
      return (
        this.chartEventYPosition 
      );
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
      return this.currentEvent.target.getBoundingClientRect().x;
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
    },
    onUpdated() {
      console.log("updated");
      this.decorateUnit();
      this.decorateTitle();
    },
  },
};
</script>

<style scoped>
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
  height: 280px;
  width: 2px;
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
</style>