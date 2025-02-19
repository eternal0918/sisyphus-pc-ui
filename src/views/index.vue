<template>
  <div class="app-container">
    <div class="boxes" v-auto-animate="{ duration: 1000,easing:'cubic-bezier(0.4, 0.0, 0.2, 1)' }">
      <EarningLineChart class="box"
                        v-for="item in numbers"
                        :key="item"
                        title="收入金额"
                        :start-color="starColor"
                        :end-color="endColor"
                        :sub-title="subTitle"
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
      subTitle: '今日收入相比较昨日上涨25%'
    };
  },
  components: {
    EarningLineChart
  },
  created() {
    for (let i = 0; i < 2; i++) {
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
  //margin: 2em -0.25em 2em -0.25em;

}

.box {
  box-sizing: border-box;
  width: calc(10% - 0.5em);
  margin: 1em;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.875em;
  font-weight: 300;
  aspect-ratio: 1;
}
</style>
