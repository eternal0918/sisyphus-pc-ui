<template>
  <div class="chart-container">
    <div style="padding: 30px;display: flex;justify-content: space-between;align-items: center">
      <span style="font-size: 18px;">模型看板</span>
      <!--      分类-->
      <div>
        <mini-tabs></mini-tabs>
      </div>
    </div>

    <div style="width: 100%;flex: 1">
      <div ref="seriesGraph" style="width: 100%;height: 100%;"/>
    </div>
  </div>
</template>


<script>
import * as echarts from 'echarts'
import MiniTabs from "@/components/MiniTabs/index.vue";

export default {
  name: "ModelStatisticsChart",
  components: {MiniTabs},
  data() {
    return {}
  },
  created() {

  },
  mounted() {
    this.initSeriesGraphData()
  },
  methods: {
    initSeriesGraphData() {
      let seriesGraphInit1 = this.seriesGraphInit()
      let seriesGraphData = {
        tooltip: {
          trigger: 'item'
        },
        color: ['#e1e1e1', '#959595','#2B2B2B'],
        legend: {
          top: '90%',
          left: 'center'
        },
        textStyle: {
          fontSize: '15px',
          fontWeight: 'bold',
          fontFamily: '汉仪正圆'
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: ['40%', '70%'],
            avoidLabelOverlap: true,
            padAngle: 5,
            itemStyle: {
              borderRadius: 10
            },
            label: {
              show: true,
              position: 'center'
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 40,
                fontWeight: 'bold'
              }
            },
            labelLine: {
              show: true
            },
            data: [
              {value: Math.random() * 1000, name: 'Search Engine'},
              {value: Math.random() * 1000, name: 'Direct'},
              {value: Math.random() * 1000, name: 'Email'}
            ],

          }
        ]
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

<style scoped lang="scss">
.chart-container {
  width: 700px;
  height: 600px;
  background: rgba(0, 0, 0, 0.02);
  border-radius: 20px;
  padding-bottom: 20px;
  display: flex;
  flex-direction: column
}
</style>
