<template>
  <div id="app">
    <label for="xcol"> X column </label>
    <select id="xcol" v-model="xCol">
      <option v-for="col of Object.keys(cols)" v-bind:key="col" v-bind:value="col">{{ cols[col] }}</option>
    </select>
    <BarChart v-bind:chartdata="chartData" v-bind:options="chartOptions"/>
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue';
export default {
  name: 'app',
  components: {
    BarChart
  },
  data: ()=>({
    dataset:{
    'date':['2019-03-01','2019-03-02','2019-03-03','2019-03-04','2019-03-05','2019-03-06','2019-03-07'],
    'dogsSeen':[1,2,3,10,5,7,9],
    'hoursSlept':[5.5,7.2,8,6.75,7.5,8,7.3],
    'coffeeDrank':[4,2,3,2,3,4,2]
    },
    cols:{
      'date':'Date',
      'dogsSeen':'Number of Dogs Seen',
      'hoursSlept':'Number of Hours Slept',
      'coffeeDrank':'Cups of Coffee Consumed'
    },
   chartOptions:{
        scales: {
            xAxes: [{
                type: 'time',
                time:{
                  displayFormats: {
                    day: 'MM/DD/YYYY'
                  },
                  unit:'day'
                }
            }]
        }
    }
  }),
  methods:{
    getChartData: function(){
      //make an array with all the keys we need 
      //then map it to the data to get the column at that index
      //to make something with shape [{col1:....,col2:....}....]
      const makeDataItem = (prev,key,idx)=>({...prev,...{[key]:this.dataset[key][idx]}});
      //make an array with the keys then make a dataitem for each index
      let chartDataSet = new Array(this.dataset[0].length)
      .fill([this.xCol,this.yCol])
      .map((keys,idx)=>(keys.reduce(makeDataItem)));
      return {
        chartData:{
          datasets: [{
              label: 'Number of Dogs Seen',
              data: chartDataSet,
              backgroundColor:0x7ceacd,
          }]
        }
      };
    },
 

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
