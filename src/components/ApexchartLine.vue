<template>
  <q-card class="q-mx-sm">
    <q-tabs
      v-model="ishourly"
      dense
      class="bg-grey-3 text-grey-7"
      active-color="primary"
      indicator-color="purple"
      align="justify"
    >
      <q-tab name="today" label="today" />
      <q-tab name="week" label="week" />
    </q-tabs>

    <div id="chart">
      <apexchart
        type="line"
        height="350"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
    <q-tab-panels v-model="ishourly" animated class="bg-primary text-white">
      <q-tab-panel name="mails">
        <div id="chart">
          <apexchart
            type="line"
            height="350"
            :options="chartOptions"
            :series="series"
          ></apexchart>
        </div>
      </q-tab-panel>

      <q-tab-panel name="alarms">
        <div id="chart">
          <apexchart
            type="line"
            height="350"
            :options="chartOptions"
            :series="series"
          ></apexchart>
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
</style>