<template>
  <div id="chart-container" class="gauge column q-mt-sm text-h2">
    <template v-if="checkValues()">
      <IEcharts :option="gauge" />
    </template>
  </div>
</template>

<script type="text/babel">
import IEcharts from "vue-echarts-v3/src/full.js";

export default {
  name: "view",
  components: {
    IEcharts,
  },
  props: {
    chartname: String,
    value: null,
  },
  data: () => ({
    loading: true,
    gauge: {
      series: [
        {
          name: "",
          type: "gauge",
          radius: "100%",
          title: {
            offsetCenter: [0, "50%"],
            fontSize: 20,
            color: "#fff",
          },
          detail: {
            fontSize: 15,
            color: "#fff",
            offsetCenter: [0, "90%"],
            formatter: "{value}",
          },

          progress: {
            show: true,
            roundCap: true,
            width: 6,
          },
          startAngle: 180,
          endAngle: 0,
          min: 0,
          max: 100,
          splitNumber: 4,
          axisLine: {
            lineStyle: {
              width: 6,
            },
          },
          axisTick: {
            length: 1,
            lineStyle: {
              color: "auto",
              width: 7,
            },
          },
          data: [
            {
              value: 0,
            },
          ],
          backgroundColor: {
            height: "200px",
          },
        },
      ],
    },
  }),
  methods: {
    checkValues() {
      console.log("called checkValues()");
      if (this.value !== null) {
        console.log("value has value " + this.value);
        this.gauge.series[0].name = this.chartname;
        this.gauge.series[0].data[0].name = this.chartname;
        this.gauge.series[0].data[0].value = this.value;
        if (this.chartname == "UV Index") {
          this.gauge.series[0].min = 0;
          this.gauge.series[0].max = 12;
          this.gauge.series[0].data[0].value = this.value;
          // this.gauge.series[0].axisLine.lineStyle.color = [
          //   [2, "#FF6E76"],
          //   [5, "#FDDD60"],
          //   [7, "#58D9F9"],
          //   [12, "#7CFFB2"],
          // ];
        } else if (this.chartname == "Sun") {
        } else {
          this.gauge.series[0].data[0].value = this.value;
        }
        return true;
      } else {
        console.log("value doesn't have value");
        this.series = [0];
        return false;
      }
    },
  },
};
</script>

<style scoped>
/* .IEcharts {
  width: 100%;
  height: 300px;
} */
#chart-container {
  position: relative;
}
</style>