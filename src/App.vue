<template>
  <div id="app">
    <div class="cols-dropdowns">
      <!-- Dropdowns to select the columns to chart -->
      <label for="xcol"> X column </label>
      <select id="xcol" v-model="xCol">
        <option v-for="col of Object.keys(cols)" v-bind:key="col" v-bind:value="col">{{ cols[col] }}</option>
      </select>

      <label for="ycol"> Y column </label>
      <select id="ycol" v-model="yCol">
        <option v-for="col of Object.keys(cols)" v-bind:key="col" v-bind:value="col">{{ cols[col] }}</option>
      </select>
    </div>

    <!-- The different charts-->
    <div class="chart-area">
      <div class="chart">
        <component v-bind:is="chartType" v-bind:chartData="chartdata" v-bind:options="chartoptions"/>
      </div>
      <div class="chart-type">
        <button v-on:click="chartType = 'BarChart'">Bar</button>
        <button v-on:click="chartType = 'LineChart'">Line</button>
      </div>
    </div>
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue';
import LineChart from './components/BarChart.vue';

export default {
  name: 'app',
  components: {
    BarChart,
    LineChart
  },
  data: ()=>({
    chartType:'BarChart',
    dataset:{
    'date':['2019-03-01','2019-03-02','2019-03-03','2019-03-04','2019-03-05','2019-03-06','2019-03-07'],
    'dogsSeen':[1,2,3,10,5,7,9],
    'hoursSlept':[5.5,7.2,8,6.75,7.5,8,7.3],
    'coffeeDrank':[4,2,3,2,3,4,2]
    },
    xCol:'date',
    yCol:'dogsSeen',
    cols:{
      'date':'Date',
      'dogsSeen':'Number of Dogs Seen',
      'hoursSlept':'Number of Hours Slept',
      'coffeeDrank':'Cups of Coffee Consumed'
    },
  }),
  computed:{
    chartdata: function(){
      //Get the x axis (labels) and y axis (data) for the chart
      //based on the columns from the dropdowns
      return {
        labels: this.dataset[this.xCol],
          datasets: [{
              label:`${this.cols[this.yCol]} vs ${this.cols[this.xCol]}`,
              data: this.dataset[this.yCol],
              backgroundColor:'#0868ad',
          }]
      };
    },
    chartoptions:function(){
      let options = {
        responsive:true,
        scales: {
          xAxes: [{
            time: {
              displayFormats:{
                day : 'DD/MM/YYYY'
              },
              unit: 'day'
            }
          }]
        }
      };
      options.scales.xAxes[0].type = this.xCol == 'date' ? 'time' : 'category';
      return options;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display:flex;
  flex-direction: column;
  padding:1em;
  width:100vw;
  height:100vh;
}

.cols-dropdowns {
  display:flex;
  margin-left:auto;
  margin-right: auto;
  flex-direction: row;
  padding:1em;
}

.cols-dropdowns #xcol{
  margin-right:1.5em;
}

.cols-dropdowns label{
  margin-right:.25em;
}

.chart-area{
  display:flex;
  flex-direction: row;
}

.chart-area .chart{
  flex:3;
}

.chart-area .chart-type{
  flex:1;
  padding:1em;
}

.chart-area .chart-type button{
  background:white;
}

</style>
