<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme  创建js文件然后引入使用
import resize from './mixins/resize'

const animationDuration = 3000

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '300px'
    }
  },
  data() {
    return {
      chart: null,
      level:[],
      leveltimes:[]
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    });
    this.getLevelData()
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons');

      this.chart.setOption({
         tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
    },
    legend: {
        orient: 'vertical',
        left: 10,
        //data: ['直接访问', '邮件营销', '联盟广告', '视频广告', '搜索引擎']
        data:this.level
    },
    series: [
        {
            name: '预警等级',
            type: 'pie',
            radius: ['50%', '70%'],
            avoidLabelOverlap: false,
            label: {
                show: false,
                position: 'center'
            },
            emphasis: {
                label: {
                    show: true,
                    fontSize: '30',
                    fontWeight: 'bold'
                }
            },
            labelLine: {
                show: false
            },
            // data: [
            //     {value: 335, name: '直接访问'},
            //     {value: 310, name: '邮件营销'},
            //     {value: 234, name: '联盟广告'},
            //     {value: 135, name: '视频广告'},
            //     {value: 1548, name: '搜索引擎'}
            // ]
            data:this.leveltimes
        }],
      
    
          animationDuration: animationDuration
      
      

       })
    },
    getLevelData(){
       var self = this;
      self.$http
        .get(this.baseUrl + "/indexfunction/level")
        .then(function(response) {
          var res = response.data;
          console.log(res);
          self.level = [];

          for (var i = 0; i < res.length; i++) {
            self.level.push(res[i].level);
          }
          self.leveltimes = [];
          //当data键值有两个或者更多时，使用循环将后台数据读取存放到数组中，然后给键赋值
          for (var i = 0; i < res.length; i++) {
            var resData = res[i];
            self.leveltimes[i] = {
              value: resData.leveltimes,
              name: resData.level
            };
          }
          self.initChart();
          console.log(self.level);
          console.log(self.leveltimes);
        });
    }
  }
}
</script>
