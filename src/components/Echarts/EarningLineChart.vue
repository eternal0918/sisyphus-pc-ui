<template>
  <div style="width: 300px;height: 180px;background: rgba(0,0,0,0.02);border-radius: 20px">
    <div ref="seriesGraph" style="width: 100%;height: 100%"/>
  </div>
</template>

<script>

import * as echarts from 'echarts'

export default {
  name: 'EarningLineChart',
  props: {
    title: {
      type: String,
      default: "标题"
    },
    titleColor: {
      type: String,
      default: 'black',
    },
    xDataValue: {
      type: Array,
      default: null
    },
    xData: {
      type: Array,
      default: null
    },
    startColor: {
      default: '#479bff',
    },
    endColor: {
      default: '#0010c6',
    }
  },
  data() {
    return {
      chartTitle: this.title,
      chartTitleColor: this.titleColor,
      chartDataValue: this.xDataValue,
      chartData: this.xData,
      chartStartColor: this.startColor,
      chartEndColor: this.endColor,
    }
  },
  created() {
    this.chartData = []
    this.chartDataValue = []
    for (let i = 1; i < 7; i++) {
      const time = '2025-2-' + i;
      this.chartData.push(time);
      this.chartDataValue.push(Math.floor(Math.random() * (50 - 15)) + 15);
    }
  },
  mounted() {
    this.initSeriesGraphData()
  },
  methods: {
    //初始化并加载数据
    initSeriesGraphData() {
      let seriesGraphInit1 = this.seriesGraphInit()
      let seriesGraphData = {
        title: {
          text: this.chartTitle,
          left: 20,
          top: 20,
          textStyle: {
            color: this.chartTitleColor
          }
        },

        tooltip: {
          trigger: 'axis'
        },
        grid: {
          left: 20,
          top: 60,
          bottom: 10,
          right: 20,
          containLabel: true
        },
        xAxis: [{
          type: 'category',
          data: this.chartData,
          axisLine: {
            show: false,
            lineStyle: {
              color: "#999"
            }
          },
          axisLabel: {
            show: false
          },
          axisTick: {
            show: false
          }
        }],
        yAxis: [{
          type: 'value',
          splitNumber: 5,
          //坐标轴虚线
          splitLine: {
            show: false,
            lineStyle: {
              type: 'dashed',
              color: '#ccc'
            }
          },
          //坐标轴刻度值
          axisLabel: {
            show: false
          },
          //坐标轴刻度线
          axisTick: {
            show: false
          },
          //坐标轴
          axisLine: {
            show: false,
            lineStyle: {
              color: "#ccc"
            },
          },
          nameTextStyle: {
            color: "#999"
          },
          splitArea: {
            show: false
          }
        }],
        series: [{
          name: '纯利润',
          type: 'line',
          data: this.chartDataValue,
          lineStyle: {
            width: 4,
            color: {
              type: 'linear',
              colorStops: [{
                offset: 0,
                color: this.chartStartColor// 0% 处的颜色
              }, {
                offset: 1,
                color: this.chartEndColor // 100% 处的颜色

              }],
              globalCoord: false // 缺省为 false
            },
            shadowColor: this.chartEndColor + 40,
            shadowBlur: 10,
            shadowOffsetY: 25,
            // shadowOffsetX: 10,

          },
          itemStyle: {
            color: '#ffffff10',
            borderWidth: 10,
            shadowColor: '#3544CF50',
            shadowBlur: 10,
            borderColor: "#3544CF"
          },
          //平滑曲线
          smooth: true
        }]
      };
      seriesGraphInit1.setOption(seriesGraphData)
    },
    //初始化
    seriesGraphInit() {
      return echarts.init(
        this.$refs.seriesGraph
      )
    }
  }
}
</script>

<style scoped>

</style>
