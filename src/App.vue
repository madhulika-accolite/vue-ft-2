<template>
  <div id="app">
    <fusioncharts
      :type="type"
      :width="width"
      :height="height"
      :dataFormat="dataFormat"
      data-empty-message="Data is loading"
      :dataSource="dataSource"
    ></fusioncharts>
  </div>
</template>

<script>
import Vue from "vue";
import VueFusionCharts from "vue-fusioncharts";
import FusionCharts from "fusioncharts";
import TimeSeries from "fusioncharts/fusioncharts.timeseries";

Vue.use(VueFusionCharts, FusionCharts, TimeSeries);
const jsonify = (res) => res.json();
const dataFetch = fetch(
  "https://s3.eu-central-1.amazonaws.com/fusion.store/ft/data/column-chart-with-time-axis-data.json"
).then(jsonify);
const schemaFetch = fetch(
  "https://s3.eu-central-1.amazonaws.com/fusion.store/ft/schema/column-chart-with-time-axis-schema.json"
).then(jsonify);

export default {
  name: "App",
  data: function () {
    return {
      type: "timeseries",
      width: "100%",
      height: "450",
      dataFormat: "json",
      dataSource: {
        chart: {
          showlegend: 0,
        },
        caption: {
          text: "Daily Visitors Count of a Website",
        },
        data: null,
        yaxis: [
          {
            plot: {
              value: "Daily Visitors",
              type: "column",
            },
            format: {
              suffix: "k",
            },
            title: "Daily Visitors Count",
          },
        ],
      },
    };
  },
  mounted: function () {
    Promise.all([dataFetch, schemaFetch]).then((res) => {
      const data = res[0];
      const schema = res[1];
      const fusionTable = new FusionCharts.DataStore().createDataTable(
        data,
        schema
      );
      this.dataSource.data = fusionTable;
    });
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
