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
import data from './assets/data';
import schema from './assets/schema'

Vue.use(VueFusionCharts, FusionCharts, TimeSeries);


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
      const fusionTable = new FusionCharts.DataStore().createDataTable(
        data,
        schema
      );
      this.dataSource.data = fusionTable;
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
