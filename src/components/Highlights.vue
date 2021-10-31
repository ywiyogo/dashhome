<template>
  <div class="col-auto row justify-around text-center">
    <div class="col-auto column">
      <svg
        class="col-auto"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        style="border-radius: 4em 4em; height: 5em; width:5em;"
      >
        <image
          width="80"
          height="80"
          color="red"
          href="~assets/weather-icons/svg/wi-humidity.svg"
          style="filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(104deg) brightness(102%) contrast(102%);"
        />
      </svg>
      <div class="col-auto text-h6">{{ p_humidity }}</div>
    </div>
    <div class="col-auto column">
      <svg
        class="col-auto"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        style="border-radius: 4em 4em; height: 5em; width:5em;"
      >
        <image
          width="80"
          height="80"
          color="red"
          href="~assets/weather-icons/svg/wi-hot.svg"
          style="filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(104deg) brightness(102%) contrast(102%);"
        />
      </svg>
      <div class="col text-h6">{{ p_uvi }}</div>
    </div>
    <div class="col-auto column">
      <svg
        class="col-auto"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        style="border-radius: 4em 4em; height: 5em; width:5em;"
      >
        <image
          width="80"
          height="80"
          color="red"
          href="~assets/weather-icons/svg/wi-strong-wind.svg"
          style="filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(104deg) brightness(102%) contrast(102%);"
        />
      </svg>
      <div class="col text-h6">{{ p_wind }} m/s</div>
    </div>
    <div class="col-auto column">
      <svg
        class="col-auto"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        style="border-radius: 4em 4em; height: 5em; width:5em;"
      >
        <image
          width="80"
          height="80"
          color="red"
          href="~assets/weather-icons/svg/wi-sunrise.svg"
          style="filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(104deg) brightness(102%) contrast(102%);"
        />
      </svg>
      <div class="col-auto text-h6">{{ p_sunnytime[0] }}</div>
    </div>
    <div class="col-auto column">
      <svg
        class="col-auto"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        style="border-radius: 4em 4em; height: 5em; width:5em;"
      >
        <image
          width="80"
          height="80"
          color="red"
          href="~assets/weather-icons/svg/wi-sunset.svg"
          style="filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(104deg) brightness(102%) contrast(102%);"
        />
      </svg>
      <div class="col-auto text-h6">{{ p_sunnytime[1] }}</div>
    </div>
  </div>
</template>

<script>
import RadialBar from "components/RadialBar.vue";
import EchartGauge from "components/EchartGauge.vue";

export default {
  name: "Highlights",
  components: {
    apexchart: VueApexCharts,
    RadialBar,
    EchartGauge,
  },
  props: {
    p_humidity: {
      type: Number,
    },
    p_wind: {
      type: Number,
    },
    p_sunnytime: null,
    p_airquality: {
      type: Number,
    },
    p_uvi: {
      type: Number,
    },
  },
  computed: {
    updateData() {
      console.log("computed Highlicht components");
      this.humidity = this.p_humidity;
      this.uvi = p_uvi;
      this.airQuality = this.p_airquality;

    },
  },
  methods: {
    update() {
      console.log("methods Highlicht components");
      this.humidity = this.p_humidity;
      this.uvi = p_uvi;
      this.airQuality = this.p_airQuality;

    },
  },
  data() {
    return {
      humidity: 0,
      airQuality: 40,
      sun: [20],
      uvi: 0,
      polarSeries: [0, 0, 0, 5, 0, 0],
      polarOptions: {
        chart: {
          type: "polarArea",
        },
        title: {
          text: "Wind",
        },
        legend: {
          show: false,
        },
        polarArea: {
          rings: {
            strokeWidth: 1,
            strokeColor: "#e8e8e8ff",
          },
          spokes: {
            strokeWidth: 1,
            connectorColors: "#e8e8e8",
          },
        },
      },
    };
  },
};
</script>

<style scoped lang="sass">
</style>