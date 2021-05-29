<template>
  <div class="row">
    <EchartGauge chartname="Humidity" :value="p_humidity"></EchartGauge>
    <EchartGauge chartname="UV Index" :value="p_uvi"></EchartGauge>
    <EchartGauge
      v-if="p_sunnytime"
      chartname="Sun"
      :value="p_sunnytime"
    ></EchartGauge>
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
    p_sunset: {
      type: Number,
    },
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