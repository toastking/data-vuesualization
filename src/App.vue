<template>
  <div id="app">
    <label for="xcol"> X column </label>
    <select id="xcol" v-model="xCol">
      <option v-for="col of Object.keys(cols)" v-bind:key="col" v-bind:value="col">{{ cols[col] }}</option>
    </select>
    <label for="ycol"> Y column </label>
    <select id="ycol" v-model="yCol">
      <option v-for="col of Object.keys(cols)" v-bind:key="col" v-bind:value="col">{{ cols[col] }}</option>
    </select>
    <BarChart v-bind:chartData="chartdata" v-bind:options="chartoptions"/>
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
      //make an array with all the keys we need 
      //then map it to the data to get the column at that index
      //to make something with shape [{col1:....,col2:....}....]
      const makeDataItem = (keys,idx)=>({
        'x':this.dataset[keys[0]][idx],
        'y':this.dataset[keys[1]][idx]
      });
      //make an array with the keys then make a dataitem for each index
      let firstCol = Object.keys(this.dataset)[0];
      let dataLength = (firstCol) ? this.dataset[firstCol].length : 0;
      let chartDataSet = new Array(dataLength)
      .fill([this.xCol,this.yCol])
      .map(makeDataItem);

      return {
          datasets: [{
              label: this.cols[this.yCol],
              data: chartDataSet,
              backgroundColor:0x7ceacd,
          }]
      };
    },
    chartoptions:function(){
      return {

        scales: {
          xAxes: [{
            type: 'time',
            time: {
              displayFormats:{
                day : 'DD MM YYYY'
              },
              unit: 'day'
            }
          }]
        }
      };
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
