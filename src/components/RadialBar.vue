<template>
  <div id="chart" class="col q-mt-xl">
    <template v-if="checkValues()">
      <apexchart
        :key="chartname"
        type="radialBar"
        width="100%"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </template>
  </div>
</template>

<script>
export default {
  name: "RadialBar",
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    chartname: String,
    value: null,
  },
  data() {
    return {
      series: [0],
      chartOptions: {
        chart: {
          type: "radialBar",
          sparkline: {
            enabled: true,
          },
        },
        title: {
          text: this.chartname,
        },
        plotOptions: {
          radialBar: {
            startAngle: -90,
            endAngle: 90,
            track: {
              background: "#e7e7e7",
              strokeWidth: "97%",
              margin: 5, // margin is in pixels
              dropShadow: {
                enabled: true,
                top: 2,
                left: 0,
                color: "#999",
                opacity: 1,
                blur: 2,
              },
            },
            dataLabels: {
              name: {
                show: false,
              },
              value: {
                offsetY: -2,
                fontSize: "22px",
              },
            },
          },
        },
        grid: {
          padding: {
            top: -10,
          },
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "light",
            shadeIntensity: 0.4,
            inverseColors: false,
            opacityFrom: 1,
            opacityTo: 1,
            stops: [0, 50, 53, 91],
          },
        },
        labels: ["Average Results"],
      },
    };
  },
  methods: {
    checkValues() {
      console.log("called checkValues()");
      if (this.value !== null) {
        console.log("value has value");
        this.series = [this.value];
        this.chartOptions.title.text = this.chartname;
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