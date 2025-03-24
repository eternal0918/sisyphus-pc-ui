<template>
  <div class="app-container">

    <!--    首页 上半部-->
    <div class="boxes" v-auto-animate="{ duration: 1000,easing:'cubic-bezier(0.4, 0.0, 0.2, 1)' }">
      <div class="statistic-box">
        <div style="flex: 1;line-height: 50px;">
          <el-row type="flex" justify="space-between">
            <el-col :span="6">
              <span style="font-size: 18px;">总计金额</span>
            </el-col>
            <el-col :span="6" style="text-align: right">
              <div class="select_btn">
                <el-select v-model="dateType">
                  <el-option
                    v-for="(item,index) in dateTypeList"
                    :key="index"
                    :label="item.label"
                    :value="item.type">
                  </el-option>
                </el-select>
              </div>
            </el-col>
          </el-row>
          <br>

          <span style="font-weight: bold;font-size: 60px;">{{ earning }}</span>
          <br>

          <h4 style="color: darkgrey;letter-spacing: 1px;font-size: 20px">{{ subTitle }}</h4>
        </div>

        <div style="display: flex;gap: 30px;height: 120px;">
          <div class="statistic-card" v-for="item in 5" :key="item">

            <div style="color: gray;display: flex;align-items: center; gap: 10px">
              <div style="width: 15px;height: 10px;background:#5B5B5B;border-radius: 3px"/>
              <span>线上金额</span>
            </div>

            <div style="display: flex;justify-content: space-between;align-items: center">
              <h2>￥{{ (Math.random() * 1000).toFixed(2) }}</h2>
              <div
                class="tips-container">
                <i class='bx bx-trending-up bx-sm bx-tada'/>+23%
              </div>
            </div>

          </div>
        </div>
      </div>

      <EarningLineChart
        v-for="item in numbers"
        :key="item"
        title="新增客户"
        :start-color="starColor"
        :end-color="endColor"
        :sub-title="subTitle"
        :number="number"
      ></EarningLineChart>
    </div>

    <!--    <button class="button button&#45;&#45;alt" @click="randomize">Randomize</button>-->
    <!--    <mini-tabs></mini-tabs>-->
    <div style="display: flex;gap: 20px">
      <model-statistics-chart></model-statistics-chart>
      <div style="flex: 1;background: rgba(0, 0, 0, 0.02);border-radius: 20px">

      </div>
    </div>

  </div>
</template>


<script>
import EarningLineChart from "@/components/Echarts/EarningLineChart.vue";
import MiniTabs from "@/components/MiniTabs/index.vue";
import ModelStatisticsChart from "@/components/Echarts/ModelStatisticsChart.vue";

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
      earning: '￥' + (Math.random() * 10000).toFixed(2),
      dateType: "week",
      dateTypeList: [
        {
          type: "year",
          label: "最近一年"
        },
        {
          type: "month",
          label: "最近一个月"
        },
        {
          type: "week",
          label: "最近一周"
        }
      ]
    };
  },
  components: {
    EarningLineChart, MiniTabs, ModelStatisticsChart
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
.select_btn {

  ::v-deep .el-select,
  ::v-deep .el-input,
  ::v-deep .el-input__inner {
    border-radius: 10px;
    color: #5B5B5B;
    border: 0px solid white;
    text-align: center;
    background: white;
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.01);
    font-weight: bold;
  }

  ::v-deep .el-input {
    border: 3px solid white;
  }

  ::v-deep .el-input.is-focus {
    border: 3px solid gainsboro;
  }

  //修改的是el-input上下的小图标的颜色

  ::v-deep .el-select .el-input .el-select__caret {
    color: #5B5B5B;
    font-weight: bold;

  }
}

</style>

<style scoped>
.boxes {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}


.statistic-box {
  flex: 1;
  background: rgba(0, 0, 0, 0.02);
  border-radius: 20px;
  height: 400px;
  padding: 30px 30px 25px 30px;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.statistic-card {
  flex: 1;
  background: white;
  border-radius: 15px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.01);
  padding: 20px 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.tips-container {
  background: #ECFAEE;
  color: #4BA143;
  padding: 8px;
  border-radius: 8px;
  font-weight: bold;
  display: flex;
  align-items: center;
  gap: 10px
}


</style>

