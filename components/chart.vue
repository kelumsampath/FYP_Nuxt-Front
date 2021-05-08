<template>
  <div class="row">
  
    <!-- Small charts -->
    <div class="col-lg-4" >
      <card type="chart">
        <template slot="header">
          <h5 class="card-category">Total Shipments</h5>
          <h3 class="card-title">
            <i class="tim-icons icon-bell-55 text-primary "></i> 763,215
          </h3>
        </template>
        <div class="chart-area">
          <line-chart
            style="height: 100%"
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
  data () {
    return {
     
      purpleLineChart: {
        extraOptions: chartConfigs.purpleChartOptions,
        chartData: {
          labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
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
              data: [80, 100, 70, 80, 120, 80]
            },{
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
              data: [23, 2, 33, 380, 120, 80]
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
