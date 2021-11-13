<template>
  <div class="max-w-7xl mx-auto sm:px-6 lg:px-4 flex flex-col">
    <Loader/>
    <div class="w-full sm:max-w-full flex justify-between items-center p-3">
      <a href="http://linkedin.com/in/loyolajuan" target="_blank" class="text-indigo-300 hover:text-indigo-400">LinkedIn</a>
      <input type="text" name="search" id="search" placeholder="Search Coin"
      @keyup="searchCoin"
      v-model="search"
      class="shadow-lg block w-2/6 h-10 sm:text-sm border-gray-200 border-2 rounded-md px-10 cursor-pointer"/>
      <a href="http://portfoliov2.hostman.site/" target="_blank" class="text-indigo-300 hover:text-indigo-400">Portfolio</a>
    </div>
    

    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
        <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-100">
              <tr>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Name
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Current Price
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Price change 24hs
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Total volume
                </th>
              </tr>
            </thead>
            <tbody class="w-full bg-white divide-y divide-gray-200">
              <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div class="flex flex-row justify-center items-center h-10 w-12">
                      <p class="">{{index + 1}}</p>
                      <img class="h-10 w-10 rounded-full mx-2" loading="lazy" :src="coin.image" alt="coin.name" />
                    </div>
                    <div class="ml-4">
                      <div class="text-sm font-medium text-gray-900 uppercase">
                        {{ coin.symbol }}
                      </div>
                      <div class="text-sm text-gray-500">
                        {{ coin.name }}
                      </div>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-gray-500">
                    ${{ coin.current_price.toLocaleString() }}
                  </div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <span
                    :class="[coin.price_change_percentage_24h > 0 ? 'bg-green-200 text-green-800' : 'bg-red-300 text-red-800']"
                    class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full">
                    {{ coin.price_change_percentage_24h }}%
                  </span>
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                    ${{ coin.total_volume.toLocaleString() }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Loader from '@/components/Loader.vue';

export default {
  name: 'App',
  data () {
    return {
      coins: [],
      filteredCoins: [],
      search: ''
    }
  },
  components: {
    Loader
  },
  async mounted () {
    const res = await fetch ('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin : function() {
      this.filteredCoins = this.coins.filter((coin) =>
        coin.name.toLowerCase().includes(this.search.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(this.search.toLowerCase())
      )
    }
  }
}
</script>
