<template>
  <canvas ref="myChart"></canvas>
</template>

<script>
import Chart from 'chart.js/auto';
export default {
    props: ['labels', 'data', 'dataLabel'],
    created() {

    },
    mounted() {
      const data = {
        labels: this.labels,
        datasets: [{
          label: this.dataLabel,
          backgroundColor: 'rgba(255, 99, 132, 0.5)',
          borderColor: 'rgb(255, 99, 132)',
          data: this.data,
        }]
      };

        const config = {
          type: 'line',
          data: data,
          options: {}
        };

        this.chart = new Chart(
          this.$refs['myChart'],
          config
        );
      },
      data() {
        return {
          chart: null
        }
      },
      watch: {
        dataLabel(newLabel) {
          this.chart.data.datasets[0].label = newLabel;
          this.chart.update();
        },
        data(newData) {
          this.chart.data.datasets[0].data = newData;
          this.chart.update();
          // console.log(this.chart);
          // console.log(newData);
        },
        labels(newLabels) {
          this.chart.data.labels = newLabels;
          this.chart.update();
        },
      },
    }
</script>

<style>

</style>
