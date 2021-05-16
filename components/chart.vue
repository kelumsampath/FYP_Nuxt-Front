<template>
  <div class="row">
  
    <!-- Small charts -->
    <div  >
      <card type="chart">
        <template slot="header">
          <h5 class="card-category">Actual VS Text Score VS Predicted</h5>
          <h3 class="card-title">
          </h3>
        </template>
        <div class="chart-area" >
          <line-chart
            style="width: 850px; height:900px"
            :chart-data="purpleLineChart.chartData"
            :gradient-colors="purpleLineChart.gradientColors"
            :gradient-stops="purpleLineChart.gradientStops"
            :extra-options="purpleLineChart.extraOptions"
          >
          </line-chart>
        </div>
      </card>
    </div>
   
  </div>
</template>
<script>
import LineChart from '@/components/LineChart';
import * as chartConfigs from '@/components/config';
import config from '@/pages/config';

export default {
  name: 'dashboard',
  components: {
    LineChart,
  },
  props: {
    lables: {
      type: Array,
      description: 'Card title'
    },
    actual: {
      type: Array,
      description: 'Card subtitle'
    },
    textScore: {
      type: Array,
      description: 'Card subtitle'
    },
    predictedSp: {
      type: Array,
      description: 'Card subtitle'
    },
  },
  data () {
    return {
     
      purpleLineChart: {
        extraOptions: chartConfigs.purpleChartOptions,
        chartData: {
          labels:this.$props.lables,
          datasets: [
            {
              label: 'Data',
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: this.$props.actual
            },{
              label: 'Data',
              fill: true,
              borderColor: config.colors.primary2,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary2,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary2,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: this.$props.textScore
            },{
              label: 'Data',
              fill: true,
              borderColor: config.colors.primary3,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary3,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary3,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: this.$props.predictedSp
            }
          ]
        },
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.2, 0]
      },
      
        
    };
  },
  computed: {
    enableRTL () {
      return this.$route.query.enableRTL;
    },
   
    bigLineChartCategories () {
      return [{ name: 'Accounts', icon: 'tim-icons icon-single-02' }, {
        name: 'Purchases',
        icon: 'tim-icons icon-gift-2'
      }, { name: 'Sessions', icon: 'tim-icons icon-tap-02' }];
    }
  },
  methods: {
  },
  mounted () {
  }
}
</script>
<style></style>
