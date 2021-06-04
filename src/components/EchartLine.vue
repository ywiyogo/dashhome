<template>
  <div class="echarts col q-mt-xl">
    <template v-if="checkValues()">
      <IEcharts :option="line" />
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
    p_hourly: null,
    p_daily: null,
    p_sun: null,
  },
  data: () => ({
    loading: true,
    line: {
      title: {
        text: "Temperatures and Rainfall",
        left: "center",
        textStyle: {
          color: "#fff",
          fontSize: 20,
        },
      },

      tooltip: {},
      xAxis: {
        name: "Hours",
        data: [],
        axisLabel: {
          fontSize: 22,
        },
      },
      yAxis: [
        {
          name: "°C",
          axisLabel: {
            fontSize: 20,
            color: "#5470c6"
          },
          splitLine:{
            lineStyle: {
              type:"dashed"
            }
          },
        },
        {
          name: "°mm",
          splitLine:{
            show: false
          },
          axisLabel: {
            fontSize: 20,
            color: "#fac858",
            
          },
        },
      ],
      series: [
        {
          name: "Temperature",
          type: "line",
          data: [],
          lineStyle: {
            width: 3,
          },
          markArea: {
            itemStyle: {
              color: "rgba(252, 248, 186, 0.2)",
            },
            data: [],
          },
        },
        {
          name: "Rainfall",
          type: "bar",
          yAxisIndex: 1,
          itemStyle: {
            color: "#fac858",
          },
          data: [],
        },
      ],
    },
  }),
  methods: {
    addHour(currHour, num) {
      let res = currHour + num;
      if (res > 23) {
        res = res % 24;
      }
      return res;
    },
    checkValues() {
      console.log("called checkValues()");
      let xdata = [];
      let ydata = [];
      let rainfall = [];
      let now = new Date();
      let hour = now.getHours();
      let _this = this;
      if (this.p_hourly !== null) {
        for (let i = 0; i < this.p_hourly.length; i++) {
          xdata.push(_this.addHour(hour, i));
          ydata.push(this.p_hourly[i].temp);
          if (this.p_hourly[i].weather[0].main === "Rain") {
            rainfall.push(this.p_hourly[i].rain["1h"]);
          } else {
            rainfall.push(0);
          }
        }
        this.line.xAxis.data = xdata;
        this.line.series[0].data = ydata;
        // use parameter 0 for epoch
        const sunriseDate = new Date(0);
        const sunsetDate = new Date(0);
        sunriseDate.setUTCSeconds(this.p_sun[0]);
        sunsetDate.setUTCSeconds(this.p_sun[1]);

        let xAxisSunRise = sunriseDate.getHours();
        let xAxisSunSet = sunsetDate.getHours();
        let showSunArea = true;
        if (hour > xAxisSunRise) {
          if (hour < sunsetDate.getHours()) {
            xAxisSunRise = 0;
            xAxisSunSet = xAxisSunSet - hour;
          } else {
            showSunArea = false;
          }
        } else {
          xAxisSunRise = xAxisSunRise - hour;
          xAxisSunSet = xAxisSunSet - hour;
        }
        // Add a sun duration area
        if (showSunArea) {
          this.line.series[0].markArea.data = [
            [
              {
                name: "Sun Duration",
                xAxis: xAxisSunRise,
              },
              {
                xAxis: xAxisSunSet,
              },
            ],
          ];
        }
        this.line.series[1].data = rainfall;
        return true;
      } else {
        console.log("WARNING: no value for the hourly data");
        this.series = [0];
        return false;
      }
    },
  },
};
</script>

<style scoped>
.echarts {
  height: 500px;
  width: 100%;
}
</style>