<template>
  <div class="container">
    <highcharts :options="chartOptions"></highcharts>
  </div>
</template>

<script>
import { EventBus } from "../utils/event-bus";

export default {
  name: 'Grafico',
  data: function() {
    return {
      chartOptions: {
        chart: {
          type: 'column',
        },
        title: {
            text: 'COVID-19: Estatística de Casos por País'
        },
        xAxis: {
            categories: ['Confirmados', 'Recuperados', 'Mortes']
        },
        yAxis: {
            title: {
                text: 'Número de Casos'
            }
        },
        series: [
          {
            name: 'País',
            data: [0, 0, 0],
            colorByPoint: true,
            colors: [
              '#FEA343',
              '#0275d8',
              '#d9534f',
            ]
          }
        ],
        credits: {
          enabled: false
        }
      }
    }
  },
  created() {
    EventBus.$on("tabela-clicked", data => {
      this.chartOptions.series[0].name = data.nomePais;
      this.chartOptions.series[0].data = data.series;
    });
  }
}

</script>

<style scoped>

</style>
