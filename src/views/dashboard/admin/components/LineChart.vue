<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";

export default {
  mixins: [resize],
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
      default: "350px"
    },
    autoResize: {
      type: Boolean,
      default: true
    }
    // chartData: {
    //   type: Object,
    //   required: true
    // }
  },
  data() {
    return {
      chart: null,
      //存放温度
      maxtemperature: [],
      mintemperature: [],

      //存放风力
      wind: [],
      //存放烟雾浓度
      smoke: []
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart();
    });
    //定义获取数据方法
    this.getTemData();
  },
  //钩子函数，将不需要的组件执行销毁
  beforeDestroy() {
    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, "macarons");
      // this.setOptions(this.chartData)

      this.chart.setOption({
        title: {
          text: "最近七天温度变化"
        },
        tooltip: {
          trigger: "axis"
        },
        legend: {
          data: ["最高气温", "最低气温"]
        },
        toolbox: {
          show: true,
          feature: {
            dataZoom: {
              yAxisIndex: "none"
            },
            dataView: { readOnly: false },
            magicType: { type: ["line", "bar"] },
            restore: {},
            saveAsImage: {}
          }
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"]
        },
        yAxis: {
          type: "value",
          axisLabel: {
            formatter: "{value} °C"
          }
        },
        series: [
          {
            name: "最高气温",
            type: "line",
            //data: [11, 11, 15, 13, 12, 13, 10],
            data: this.maxtemperature,
            markPoint: {
              data: [
                { type: "max", name: "最大值" },
                { type: "min", name: "最小值" }
              ]
            },
            markLine: {
              data: [{ type: "average", name: "平均值" }]
            }
          },
          {
            name: "最低气温",
            type: "line",
            // data: [1, -2, 2, 5, 3, 2, 0],
            data: this.mintemperature,
            markPoint: {
              data: [{ name: "周最低", value: -2, xAxis: 1, yAxis: -1.5 }]
            },
            markLine: {
              data: [
                { type: "average", name: "平均值" },
                [
                  {
                    symbol: "none",
                    x: "90%",
                    yAxis: "max"
                  },
                  {
                    symbol: "circle",
                    label: {
                      position: "start",
                      formatter: "最大值"
                    },
                    type: "max",
                    name: "最高点"
                  }
                ]
              ]
            }
          }
        ]
      });
    },
    //定义返回数据的方法
    getTemData() {
      var self = this;
      self.$http
        .get(this.baseUrl + "/indexfunction/all")
        .then(function(response) {
          //获取数据
          var res = response.data;
          console.log(res);
          self.maxtemperature = [];
          self.mintemperature = [];
          for (var i = 0; i < res.length; i++) {
            self.maxtemperature.push(res[i].maxTemperature);
          }
          for (var i = 0; i < res.length; i++) {
            self.mintemperature.push(res[i].minTemperature);
          }
          self.initChart();
          console.log(self.minTemperature);
        });
    }
  }
};
</script>
