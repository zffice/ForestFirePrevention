<template>
    <div :id="id" :class="className" :style="{height:height,width:width}" />
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
    id: {
      type: String,
      default: "chart"
    },
    width: {
      type: String,
      default: "200px"
    },
    height: {
      type: String,
      default: "200px"
    }
  },
  data() {
    return {
      chart: null
      // emdata: [],
      // emtype: []
    };
  },

  mounted() {
    this.initChart();
    // this.getEqData()
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
       this.chart = echarts.init(document.getElementById(this.id));
      
      this.chart.setOption({
        backgroundColor: "#344b59",

        title: {
          text: "设备在线情况统计",

          x: "20",
          top: "20",
          textStyle: {
            color: "#F1F1F3",
            fontSize: "22"
          },
          subtextStyle: {
            color: "#90979c",
            fontSize: "16"
          },
          left: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: 10,
          data: [
            "zigbee",
            "温度传感器",
            "湿度传感器",
            "烟雾浓度传感器",
            "网络摄像头"
          ]
        },
        series: [
          {
            name: "设备在线",
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center"
            },
            emphasis: {
              label: {
                show: true,
                fontSize: "30",
                fontWeight: "bold"
              }
            },
            labelLine: {
              show: false
            },
            data: [
              { value: 335, name: "zigbee" },
              { value: 310, name: "温度传感器" },
              { value: 234, name: "湿度传感器" },
              { value: 135, name: "烟雾浓度传感器" },
              { value: 1548, name: "网络摄像头" }
            ]
          }
        ]
      });
    }
  }
};
</script>

