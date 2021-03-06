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
        <button v-for="(text,type) of chartTypes" 
        v-bind:class="['chart-type-button',{ active: chartType == type }]" 
        v-bind:key="type" 
        v-on:click="chartType = type">
          {{ text }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import patternomaly from 'patternomaly';
import BarChart from './components/BarChart.vue';
import LineChart from './components/LineChart.vue';
import ScatterChart from './components/ScatterChart.vue';
import PieChart from './components/PieChart.vue';

const chartColors = ['#41b883','#35495e','#54457f','#f5ee9e','#fe5f55','#ea8c55','#5f0f40'];

export default {
  name: 'app',
  components: {
    BarChart,
    LineChart,
    ScatterChart,
    PieChart
  },
  data: ()=>({
    chartType:'BarChart',
    chartTypes:{
      'BarChart':'Bar',
      'LineChart':'Line',
      'ScatterChart':'Scatter',
      'PieChart':'Pie',
    },
    dataset:{
    'date':['2019-03-01','2019-03-02','2019-03-03','2019-03-04','2019-03-05','2019-03-06','2019-03-07'],
    'dogsSeen':[20,21,22,21,22,21,20],
    'hoursSlept':[5.5,9,7.2,6.75,7.5,7.25,7.3],
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
      let datapoints = null;
      const dataSet = {
          datasets: [{
              label:`${this.cols[this.yCol]} vs ${this.cols[this.xCol]}`,
              backgroundColor: patternomaly.generate(chartColors),
              borderColor:'#0868ad',
              fill:false,
          }]
      };

      if(this.chartType == 'ScatterChart'|| this.chartType == 'LineChart'){
        //make the data into points 
        //and sort them so the x axis increases
        datapoints = [];
        for(let i = 0; i < this.dataset[this.xCol].length; i++){
          datapoints.push({ 'x':this.dataset[this.xCol][i], 'y': this.dataset[this.yCol][i] });
        }
        dataSet.datasets[0].data = datapoints.sort((d1,d2)=>(d1.x-d2.x));
      }else{
        //otherwise pass in arrays
        dataSet.labels= this.dataset[this.xCol];
        dataSet.datasets[0].data = this.dataset[this.yCol];
      }

      return dataSet;
    },
    chartoptions:function(){
      let options = {
        responsive:true,
        maintainAspectRatio:false,
        scales: {
          xAxes: [{}],
          yAxes: [{
            ticks: {
              beginAtZero: this.chartType == 'BarChart', 
            }
          }],
        }
      };

      if(this.xCol == 'date'){
        //special case the date column to handle time axes
        options.scales.xAxes[0] = Object.assign(options.scales.xAxes[0],{
          type:'time',
          time:{
            displayFormats:{
              day : 'MM/DD/YYYY'
            },
            unit: 'day'
          }
        });
      }else if(this.chartType == 'LineChart'  || this.chartType == 'ScatterChart'){
        //special case the line/scatter plot to have a linear axis
        options.scales.xAxes[0].type='linear';
      }
      return options;
    }
  }
}
</script>

<style>
body{
  padding:0;
  margin:0;
}

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
  box-sizing: border-box;
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
  max-height:90vh;
}

.chart-area .chart-type{
  flex:1;
  flex-direction: column;
  padding:1em;
}

/* style the buttons*/
.chart-area .chart-type .chart-type-button{
  background:white;
  border-radius: .1em;
  border: 1px solid #35495e;
  padding: .5em;
  margin-top:.5em;
  margin-bottom:.5em;
  font-size:1.5em;
  display:block;
  flex:1;
  width:100%;
  cursor:pointer;
}


.chart-area .chart-type .chart-type-button.active{
  background:#41b883;
  color:white;
  border:white;
}

</style>
