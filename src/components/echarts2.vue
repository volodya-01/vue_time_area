<template>
  <div :class="className" :style="{height:height,width:width}"/>
</template>

<script>
import Bus from "@/bus.js";
import { debounce } from "@/utils";
export default {
  name: "Echarts2",
  props: {
    className: {
      type: String,
      default: "chart"
    },
    width: {
      type: String,
      default: "100%"
    },
    height: {
      type: String,
      default: "10px"
    },
    autoResize: {
      type: Boolean,
      default: true
    }
    /*  chartData: {
      type: Object,
      required: true
    } */
  },
  data() {
    return {
      chart: null,
      sidebarElm: null,
      chartData: {}
    };
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val);
      }
    }
  },
  mounted() {
    Bus.$on("defaltalltimetablefun", e => {
      var xdat = [];
      for (var i = 0; i < 1440; i++) {
        xdat.push(i);
      }
      var dat = e;
      var a = {xdat,dat};
       this.chartData = a;
      this.$nextTick(this.initChart());
    });
    if (this.autoResize) {
      this.__resizeHandler = debounce(() => {
        if (this.chart) {
          this.chart.resize();
        }
      }, 100);
      window.addEventListener("resize", this.__resizeHandler);
    }

    // 监听侧边栏的变化
    this.sidebarElm = document.getElementsByClassName("sidebar-container")[0];
    this.sidebarElm &&
      this.sidebarElm.addEventListener(
        "transitionend",
        this.sidebarResizeHandler
      );
  },
  beforeDestroy() {
    if (!this.chart) {
      return;
    }
    if (this.autoResize) {
      window.removeEventListener("resize", this.__resizeHandler);
    }

    this.sidebarElm &&
      this.sidebarElm.removeEventListener(
        "transitionend",
        this.sidebarResizeHandler
      );

    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    sidebarResizeHandler(e) {
      if (e.propertyName === "width") {
        this.__resizeHandler();
      }
    },
    setOptions(chartData) {
      this.chart.setOption({
        grid: {
          height:8,
        /*   width: 760, */
         /*  bottom: 10, */
          top: 2, 
          right: 0,
          left: 0,
          show: true,
          /*    backgroundColor: "#e5e8ec", */
          borderColor: "#dfdfe1",
          borderWidth: 1
        },
        /*  tooltip: {
          trigger: "axis",
          padding: [0, 10],
          confine: true,
          formatter: function(params) {
            console.log("params");
            console.log(params);
            var res = "<div> <p> 时间：" + params[0].name + " </p> </div>";
            for (var i = 0; i < params.length; i++) {
              if (params[i].seriesName == "历史压力") {
                res +=
                  "<p>" + params[i].seriesName + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "控制压力") {
                res +=
                  "<p>" + params[i].seriesName + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "实际压力") {
                res += "<p>" + "实际压力" + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "预测压力") {
                res += "<p>" + "预测压力" + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "上下限") {
                res += "<p>" + "上限" + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "下限") {
                res += "<p>" + "下限" + ":" + params[i].data + "</p>";
              } else if (params[i].seriesName == "调度预测压力") {
                res +=
                  "<p>" +
                  "调度时间:" +
                  chartData.Result +
                  "<br>" +
                  "调度预测压力" +
                  ":" +
                  params[i].data[1] +
                  "</p>";
              }
            }
            return res;
          },
          textStyle: {
            align: "left"
          }
        }, */
        xAxis: [
          {
            data: chartData.xdat,
            axisLabel: {
               show: false,
              inside: false,
              textStyle: {
                color: "#fff"
              }
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: false
            },
            z: 10
          }
        ],
        yAxis: {
          splitLine: {
            show: false,
            lineStyle: {
              color: "#dfdfdf",
              width: 1,
              type: "dashed"
            }
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          axisLabel: {
             show: false,
            textStyle: {
              color: "#999"
            }
          }
        },
        series: [
          {
            name: "时间",
            type: "bar",
            symbol: "none",
            data: chartData.dat,
            barWidth:'1',
            barCategoryGap:'0%',
            itemStyle: {
              normal: {
                color: "#9dc54e"
              }
            }
          }
        ]
      });
    },
    initChart() {
      this.chart = this.$echarts.init(this.$el, "macarons");
      this.chart.clear();
      this.setOptions(this.chartData);
    }
  }
};
</script>
