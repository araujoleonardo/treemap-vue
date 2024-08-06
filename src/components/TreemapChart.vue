<template>
  <div id="canvas">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script setup>
import {ref, watch} from 'vue';
import { TreemapController, TreemapElement } from 'chartjs-chart-treemap';
import Chart from 'chart.js/auto';

Chart.register(TreemapController, TreemapElement);

const canvas = ref(null);

const props = defineProps({
  data: { type: [Array], default: [] },
});

const treemap = (coin) => {
  const ctx = canvas.value.getContext('2d');

  new Chart(ctx, {
    type: 'treemap',
    data: {
      datasets: [
        {
          label: 'Criptomoedas',
          tree: coin,
          key: 'value',
          groups: ['name']
        }
      ]
    },
    options: {
      plugins: {
        tooltip: {
          callbacks: {
            label: (context) => {
              return `${context.raw.g}: ${context.raw.v}`;
            }
          }
        }
      }
    }
  });
}

watch(() => props.data, (newCoins) => {
  if (newCoins) {
    treemap(newCoins);
  }
});
</script>

<style scoped>
  #canvas {
    width: 80%;
  }
</style>