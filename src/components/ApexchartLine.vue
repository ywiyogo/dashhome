<template>
  <q-card class>
    <div id="chart">
      <apexchart type="line" height="350" :options="chartOptions" :series="series"></apexchart>
    </div>
    <q-tab-panels v-model="ishourly" animated class="bg-primary text-white">
      <q-tab-panel name="mails">
        <div id="chart">
          <apexchart type="line" height="350" :options="chartOptions" :series="series"></apexchart>
        </div>
      </q-tab-panel>

      <q-tab-panel name="alarms">
        <div id="chart">
          <apexchart type="line" height="350" :options="chartOptions" :series="series"></apexchart>
        </div>
      </q-tab-panel>
    </q-tab-panels>
  </q-card>
</template>

<script>
export default {
  name: "ApexchartLine",
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    ishourly: true,
    highData: [],
    lowData: [],
  },
  data() {
    return {
      series: [
        {
          name: "High - 2013",
          data: [],
        },
        {
          name: "Low - 2013",
          data: [],
        },
      ],
      chartOptions: {
        chart: {
          height: 350,
          type: "line",
          dropShadow: {
            enabled: true,
            color: "#000",
            top: 18,
            left: 7,
            blur: 10,
            opacity: 0.2,
          },
          toolbar: {
            show: false,
          },
          background: "#000"
        },
        colors: ["#77B6EA", "#545454"],
        dataLabels: {
          enabled: true,
        },
        stroke: {
          curve: "smooth",
        },
        title: {
          text: "Average High & Low Temperature",
          align: "left",
        },
        grid: {
          borderColor: "#e7e7e7",
          row: {
            colors: ["#f3f3f3", "transparent"], // takes an array which will be repeated on columns
            opacity: 0.5,
          },
        },
        markers: {
          size: 1,
        },
        xaxis: {
          categories: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul"],
          title: {
            text: "Month",
          },
        },
        yaxis: {
          title: {
            text: "Temperature",
          },
          min: 5,
          max: 40,
        },
        legend: {
          position: "top",
          horizontalAlign: "right",
          floating: true,
          offsetY: -25,
          offsetX: -5,
        },
      },
    };
  },
};
</script>

<style scoped lang="sass">
.q-page
  max-height: 100vh
  min-height: 100vh
  background: linear-gradient(to left, #fc5c7d, #6a82fb) /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  &.backgrnd-night
    background: linear-gradient(to bottom, #232527, #414346)
  &.backgrnd-day
    background: linear-gradient(to bottom, #00b09b, #96c93d)
</style>