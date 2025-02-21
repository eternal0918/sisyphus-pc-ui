<template>
  <div class="app-container">
    <div class="boxes" v-auto-animate="{ duration: 1000,easing:'cubic-bezier(0.4, 0.0, 0.2, 1)' }">
      <div
        style="flex: 1; background: rgba(0,0,0,0.02);border-radius: 20px;height: 320px;padding: 30px 30px 0;">
        <el-row type="flex" justify="space-between">
          <el-col :span="6">
            <span style="font-size: 18px;">总计金额</span>
          </el-col>
          <el-col :span="6" style="text-align: right">
            <el-button type="primary" icon="el-icon-more"
                       style="border: none;background:  rgba(0,0,0,0.05);color: black;"/>
          </el-col>
        </el-row>
        <br>
        <span style="font-weight: bold;font-size: 30px;">{{ earning }}</span>
        <div style="display: flex;">
          <div style="flex: 1"></div>
          <div style="flex: 1"></div>
          <div style="flex: 1"></div>
        </div>
      </div>

      <EarningLineChart class="box"
                        v-for="item in numbers"
                        :key="item"
                        title="新增客户"
                        :start-color="starColor"
                        :end-color="endColor"
                        :sub-title="subTitle"
                        :number="number"
      ></EarningLineChart>
    </div>
    <button class="button button--alt" @click="randomize">Randomize</button>
  </div>
</template>


<script>
import EarningLineChart from "@/components/Echarts/EarningLineChart.vue";

export default {
  name: "Index",
  data() {
    return {
      // 版本号
      version: "3.6.5",
      numbers: [],
      flag: true,
      starColor: '#676767',
      endColor: '#000000',
      subTitle: '今日新增客户相比较昨日上涨25%',
      number: '3,242 人',
      earning: '￥1,992,342'
    };
  },
  components: {
    EarningLineChart
  },
  created() {
    for (let i = 0; i < 1; i++) {
      this.numbers.push(i);
    }
  },
  methods: {

    randomize() {
      if (this.flag) {
        this.numbers.sort(() => (Math.random() > 0.5 ? 1 : -1))
      }
    }
  },
};
</script>

<style scoped lang="scss">
</style>

<style scoped>
.boxes {
  display: flex;
  flex-wrap: wrap;
  /**
  margin: 2em -0.25em 2em -0.25em;
   */

}

.box {
  box-sizing: border-box;
  width: calc(10% - 0.5em);
  margin: 0 1em 1em 1em;
  display: flex;
  align-items: center;
  justify-content: center;
  /**
  按比例设置宽高
   */
  aspect-ratio: 1;
}
</style>
