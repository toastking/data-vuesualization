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
        <button v-for="(text,type) of chartTypes" v-bind:class="['chart-type-button',{ active: chartType == type }]" v-bind:key="type" v-on:click="chartType = type">{{ text }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue';
import LineChart from './components/LineChart.vue';

export default {
  name: 'app',
  components: {
    BarChart,
    LineChart
  },
  data: ()=>({
    chartType:'BarChart',
    chartTypes:{
      'BarChart':'Bar',
      'LineChart':'Line',
    },
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
              borderColor:'#0868ad',
              fill:false,
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
                day : 'MM/DD/YYYY'
              },
              unit: 'day'
            }
          }],
          yAxes: [{
            ticks: {
              beginAtZero: true, 
            }
          }],
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
  width:90vw;
  height:90vh;
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
