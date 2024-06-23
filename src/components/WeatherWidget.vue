<template>
  <div class="weather-widget">
    <h2>Weather Widget</h2>
    <div class="search-bar">
      <input v-model="city" placeholder="Enter city name" @keyup.enter="getWeather" />
      <button @click="getWeather">Search</button>
    </div>
    <div v-if="weather" class="weather-info">
      <div class="weather-item">
        <p><strong>Location:</strong> {{ weather.name }}</p>
        <img src="../assets/loc.png" alt="location icon" />
        <div class="weather-border"></div>
      </div>
      <div class="weather-item">
        <p><strong>Temperature:</strong> {{ weather.main.temp }}°C</p>
        <img src="../assets/temp.png" alt="temperature icon" />
        <div class="weather-border"></div>
      </div>
      <div class="weather-item">
        <p><strong>Weather:</strong> {{ weather.weather[0].description }} </p>
        <img :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}.png`" alt="weather icon" />
        <div class="weather-border"></div>
      </div>
    </div>
    <div v-else>
      <p>Enter a city name to see the weather.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const city = ref('')
const weather = ref(null)

const getWeather = async () => {
  const apiKey = '23f0987b01236b80c30ceeb06f7c8c22'
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${apiKey}`

  try {
    const response = await axios.get(url)
    weather.value = response.data
  } catch (error) {
    console.error(error)
    weather.value = null
  }
}
</script>

<style scoped>
:root {
  --dark-gray: #2f2f2f;
  --medium-gray: #5a5a5a;
  --light-gray: #b0b0b0;
  --lightest-gray: #f0f0f0;
}

.weather-widget {
  padding: 20px;
  border: 1px solid var(--medium-gray);
  border-radius: 10px;
  max-width: 750px;
  margin: 50px auto;
  text-align: center;
  background-color: var(--lightest-gray);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.weather-widget h2 {
  color: var(--dark-gray);
  font-size: 36px;
  font-weight: bold;
  margin-bottom: 20px;
}

.search-bar {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.search-bar input {
  padding: 10px;
  width: 70%;
  margin-right: 10px;
  border: 1px solid var(--medium-gray);
  border-radius: 5px;
  background-color: var(--light-gray);
}

.search-bar button {
  padding: 10px;
  border: 1px solid var(--dark-gray);
  background-color: var(--medium-gray);
  color: var(--lightest-gray);
  cursor: pointer;
  border-radius: 5px;
}

.search-bar button:hover {
  background-color: var(--dark-gray);
}

.weather-info {
  margin-top: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.weather-item {
  margin: 10px;
  border: 1px solid var(--medium-gray);
  border-radius: 5px;
  padding: 10px;
  flex: 1;
  background-color: var(--light-gray);
  text-align: center;
  width: 200px;
}

.weather-item img {
  margin-top: 10px;
  width: 35px;
  height: 35px;
}

.weather-item p {
  margin: 0;
  color: var(--dark-gray);
}

.weather-border {
  border-top: 1px solid var(--medium-gray);
  margin-top: 10px;
  padding-top: 10px;
}
</style>
