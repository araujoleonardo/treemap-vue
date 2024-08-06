<template>
  <div>
    <h3>Toplist por volume últimas 24H</h3>
    Top 10 Crypto

    <TreemapChart :data="coins"/>
  </div>
</template>

<script setup>
  import api from "@/plugins/axios.js";
  import TreemapChart from "@/components/TreemapChart.vue";
  import {computed, onMounted, ref} from "vue";

  let data = ref([]);

  onMounted(() => {
    getData();
  });

  const getData = () => {
    api.get('data/top/totalvolfull?limit=30&tsym=USD').then((response) => {

      data.value = response.data.Data;

    }).catch((error) => {

      console.log(error);

    });
  }

  const coins = computed(() => {
    return data.value.map(coin => {
      const price = coin.RAW?.USD?.PRICE ?? 0;
      return {
        name: coin.CoinInfo.Name,
        value: (price / 1000).toFixed(2)
      };
    });
  });
</script>