<script>
    //Import the Bar component to extend
    import {Line,mixins} from 'vue-chartjs';
    // mixin to make the chartData prop reactive
    const { reactiveProp } = mixins; 

    export default {
        extends:Line,
        props:['options'],
        mixins: [reactiveProp],
        mounted () {
            //render the ChartJS chart on mount
            this.renderChart(this.chartData, this.options);
        }
    }
</script>
