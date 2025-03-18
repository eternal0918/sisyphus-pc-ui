<template>
  <div
    style="background: rgba(0,0,0,0.02);border-radius: 20px;width: 500px;max-height: 320px;padding: 30px 30px 0;display: flex;flex-direction: column;">
    <div style="width: 100%;">
      <el-row type="flex" justify="space-between">
        <el-col :span="6">
          <span style="font-size: 18px;">
            {{ chartTitle }}
          </span>
        </el-col>
        <el-col :span="6" style="text-align: right">
          <el-button  icon="el-icon-more"
                     style="border: none;background:  rgba(0,0,0,0.05);color: black;border-radius: 10px"/>
        </el-col>
      </el-row>

      <div style="display: flex;align-items: center;gap: 10px">
        <span style="font-weight: bold;font-size: 25px;margin-right: 10px">
          {{ number }}
        </span>
<!--        <el-button icon="el-icon-top-right"-->
<!--                   size="small"-->
<!--                   style="border: none;background: #ECFAEE;color: green;padding: 10px;border-radius: 8px;font-weight: bold">+23%-->
<!--        </el-button>-->
        <div
          style="background: #ECFAEE;color: #4BA143;padding: 8px;border-radius: 8px;font-weight: bold;display: flex;align-items: center; gap: 10px">
          <i class='bx bx-trending-up  bx-tada'/>
          <span>+23%</span>
        </div>
      </div>
      <br>
      <span
        style="color: darkgrey;font-weight: bold;font-size: 13px;letter-spacing: 1px">{{ subTitle }}</span>
    </div>
    <div style="width: 500px;flex: 1">
      <div ref="seriesGraph" style="width: 100%;height: 100%"/>
    </div>
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
    },
    subTitle: {
      type: String,
    },
    number: {
      type: String,
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
        // title: {
        //   text: this.chartTitle,
        //   left: 20,
        //   top: 20,
        //   textStyle: {
        //     color: this.chartTitleColor
        //   }
        // },

        tooltip: {
          trigger: 'axis'
        },
        grid: {
          left: 0,
          top: 40,
          bottom: 10,
          right: 0,
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
