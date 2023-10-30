<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for a city or state" class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">
  <ul class="absolute bg-weather-secondary text-white shadow-md py-2 px-1 top-[66px] w-full" v-if="apiSearchResult">
    <li class="py-2 cursor-pointer">
      {{ apiSearchResult.name }}, {{ apiSearchResult.sys.country }}
    </li>
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

  const api_key = 'e67840bae93839e21012aedf830ab540'
  const url_base = 'https://api.openweathermap.org/data/2.5/' 

const getSearchResults = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(`${url_base}weather?q=${searchQuery.value}&units=metric&lang=es&appid=${api_key}`)
        apiSearchResult.value = result.data
      } catch (error) {
        console.log("Error al obtener la ciudad")
      }
    } else {
      apiSearchResult.value = null
    }
  }, 300)
  }
</script>
