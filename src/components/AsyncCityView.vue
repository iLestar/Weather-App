<template>
    <div>
    </div>
</template>

<script setup>
import axios from 'axios'
import { useRoute } from 'vue-router'

const owAPIKey = 'e67840bae93839e21012aedf830ab540'
const route = useRoute()

const getWeatherData = async () => {
    try {
        const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&appid=${owAPIKey}&units=metric`)
        return weatherData.data
    } catch (error) {
        console.log(error);
    }
}
const weatherData = await getWeatherData();
console.log(weatherData);
</script>
