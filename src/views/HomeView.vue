<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">
      <ul class="absolute bg-weather-secondary text-white shadow-md py-2 px-1 top-[66px] w-full" v-if="apiSearchResult || (apiSearchResult === null && searchError === true)">
        <p class="py-2" v-if="searchError === true">Sorry, no result matches, try a different term.</p>
        <template v-else>
          <li class="py-2 cursor-pointer">
            {{ apiSearchResult.name }}, {{ apiSearchResult.sys.country }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios'

const searchQuery = ref("")
const queryTimeout = ref("null")
const apiSearchResult = ref(null)
const searchError = ref(null)

const api_key = 'e67840bae93839e21012aedf830ab540'
const url_base = 'https://api.openweathermap.org/data/2.5/'

const getSearchResults = () => {
  clearTimeout(queryTimeout.value)
  searchError.value = null
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(`${url_base}weather?q=${searchQuery.value}&units=metric&lang=es&appid=${api_key}`)
        apiSearchResult.value = result.data
      } catch (error) {
        searchError.value = true
      }
    } else {
      apiSearchResult.value = null
    }
  }, 300)
}
</script>
