<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: ['January', 'February', 'March', 'April'],
        datasets: [
          {
            label: 'Your Data',
            data: [528, 757, 888, 896],
            borderColor: '#7856ff',
            backgroundColor: '#160634',
          },
          {
            label: 'Our Data',
            data: [582, 808, 999,1010],
            borderColor: '#7856ff',
            backgroundColor: '#9881fd',
          }, 
          {
            label: 'Projected Data',
            data: [620, 900, 1111, 1200],
            borderColor: 'rgba(0, 0, 0, 0.1)',
            backgroundColor: 'rgba(115, 86, 254, 0.3)',
          }
        ]
      },
      chartOptions: {
        responsive: true
      }
    }
  }
}
</script>
