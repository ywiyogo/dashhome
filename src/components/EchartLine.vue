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
          fontSize: 15,
        },
      },

      tooltip: {},
      xAxis: {
        name: "Hours",
        data: [],
        axisLabel: {
          fontSize: 18,
        },
      },
      yAxis: {
        name: "°C or mm",
        axisLabel: {
          fontSize: 18,
        },
      },
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
        console.log("value has value");
        for (let i = 0; i < this.p_hourly.length; i++) {
          xdata.push(_this.addHour(hour, i));
          ydata.push(this.p_hourly[i].temp);
          if (this.p_hourly[i].weather.main === "Rain") {
            rainfall.push(this.p_hourly[i].rain);
          } else {
            rainfall.push(0.1);
          }
        }
        this.line.xAxis.data = xdata;
        this.line.series[0].data = ydata;
        // use parameter 0 for epoch
        const sunriseDate = new Date(0);
        const sunsetDate = new Date(0);
        sunriseDate.setUTCSeconds(this.p_sun[0]);
        sunsetDate.setUTCSeconds(this.p_sun[1]);

        console.log(
          "sunrise: " +
            sunriseDate.getHours() +
            " sunset: " +
            sunsetDate.getHours()
        );
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
          console.log(this.line.series[0].markArea.data);
        }

        this.line.series[1].data = rainfall;
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
.echarts {
  height: 500px;
  width: 100%;
}
</style>