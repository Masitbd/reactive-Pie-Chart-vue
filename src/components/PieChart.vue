<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue';
import Chart from 'chart.js/auto'

const fetchData = async () => {
  try {
    const response = await fetch('/data.json'); 
    const data = await response.json();
    createChart(data); 
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

const createChart = (data) => {
  const chart = chartCanvas.value.getContext('2d');
  const pieChart = new Chart(chart, {
    type: 'pie', 
    data: {
      labels: data.labels,
      datasets: [
        {
          data: data.values,
          backgroundColor: data.colors,
          hoverOffset: 5
        },
      ],
    },
  });

  chartInstance.value = pieChart;
};

const chartInstance = ref(null);
const chartCanvas = ref(null);

onMounted(() => {
  fetchData(); 
});

onBeforeUnmount(() => {
 
  if (chartInstance.value) {
    chartInstance.value.destroy();
  }
});
</script>
<template>
  <div class="pie-chart">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<style scoped>
.pie-chart {
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
}
</style>
