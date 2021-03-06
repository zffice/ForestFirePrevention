<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";

const animationDuration = 6000;

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
      //存放湿度
      humidity: []
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart();
    });
    this.getHumDdata();
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
        formatter: '{a} <br/>{b} : {c}%'
    },
    toolbox: {
        feature: {
            restore: {},
            saveAsImage: {}
        }
    },
    series: [
        {
            name: '湿度表',
            type: 'gauge',
            detail: {formatter: '{value}%'},
            data: [{value: this.humidity, name: '湿度'}]
        }
    ]
      });
    },
    //使用axios获取后台数据
    getHumDdata() {
      var self = this;
      self.$http
        .get(this.baseUrl + "/indexfunction/all")
        .then(function(response) {
          //获取数据
          var res = response.data;
          console.log(res);
          self.humidity = [];

          for (var i = 0; i < res.length; i++) {
            self.humidity.push(res[i].humidity);
          }

          self.initChart();
          console.log(self.humidity);
        });
    }
  }
};
</script>
