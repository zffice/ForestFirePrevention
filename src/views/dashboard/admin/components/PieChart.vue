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
      default: "300px"
    }
  },
  data() {
    return {
      chart: null,
      emdata: [],
      emtype: []
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart();
    });
    this.getEmData();
  },
  beforeDestroy() {
    if (!this.chart) {
      return;
    }
    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, "macarons");

      this.chart.setOption({
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          left: "center",
          bottom: "10",

          data: this.emdata
        },
        series: [
          {
            name: "WEEKLY WRITE ARTICLES",
            type: "pie",
            roseType: "radius",
            radius: [15, 95],
            center: ["50%", "38%"],

            data: this.emtype,
            animationEasing: "cubicInOut",
            animationDuration: 2600
          }
        ]
      });
    },
    getEmData() {
      var self = this;
      self.$http
        .get(this.baseUrl + "/indexfunction/emergrncy")
        .then(function(response) {
          var res = response.data;
          console.log(res);
          self.emdata = [];

          for (var i = 0; i < res.length; i++) {
            self.emdata.push(res[i].type);
          }
          self.emtype = [];
          //当data键值有两个或者更多时，使用循环将后台数据读取存放到数组中，然后给键赋值
          for (var i = 0; i < res.length; i++) {
            var resData = res[i];
            self.emtype[i] = {
              value: resData.typetimes,
              name: resData.type
            };
          }
          self.initChart();
          console.log(self.emdata);
          console.log(self.emtype);
        });
    }
  }
};
</script>
