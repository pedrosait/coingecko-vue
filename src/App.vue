<script>
export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ['#','Coin','Price','Price change','24h volume'],
      textSearch: ''
    }
  },
  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
    const json = await res.json();
    this.coins = json;
    this.filteredCoins = json;
  },
  methods: {
    searchCoin() {
     this.filteredCoins = this.coins.filter((coin) => coin.name.toLowerCase().includes(this.textSearch.toLocaleLowerCase()) ||
     coin.symbol.toLowerCase().includes(this.textSearch.toLocaleLowerCase()));
    }
  }

  
}
</script>

<template>
  <div class="container">
    <h1 class="text-center mt-3">Coin Market</h1>
    <input 
      type="text" 
      class="form-control bg-dark text-white rounded-0 border-0 my-4" 
      placeholder="Search coin"
      @keyup="searchCoin()"
      v-model="textSearch"
      >

  <table class="table table-dark">
    <thead>
      <tr>
        <th v-for="title in titles" :key="title">{{ title }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
        <td>
          <tr class="text-muted">{{++index}}</tr>
        </td>
        <td> 
          <img :src="coin.image" style="width:2rem" class="me-2">
          <span>{{ coin.name }}</span>
          <span class="ms-2 text-uppercase text-muted">{{ coin.symbol }}</span>
        </td>
        <td>
          $ {{coin.current_price}}
        </td>
        <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
          {{coin.price_change_percentage_24h}} %
        </td>
        <td>
          $ {{coin.total_volume.toLocaleString()}}
        </td>
      </tr>
    </tbody>
  </table>
  </div>
  
</template>

<style>

</style>
