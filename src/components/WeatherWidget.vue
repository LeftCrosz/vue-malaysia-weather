<script setup lang="ts">
import { ref } from 'vue'

const location = ref('')
const result = ref('Search Malaysia weather with MET Realtime API')
const loading = ref(false)
const date = ref(new Date().toLocaleDateString())

async function fetchWeather() {
  if (location.value.trim() !== '') {
    result.value = 'Loading...'
    try {
      const res = await fetch(
        `https://api.data.gov.my/weather/forecast?contains=${location.value}@location__location_name&${date.value}@date&limit=1`,
      )
      const data = await res.json()
      if (data.length > 0) {
        const forecast = data[0]
        result.value = `
        Location: ${forecast.location.location_name}
        Min Temp: ${forecast.min_temp}°
        Max Temp ${forecast.max_temp}°`
      } else {
        result.value = 'No forescast info found.'
      }
    } catch (error) {
      result.value = 'Error: ' + error
    } finally {
      loading.value = false
    }
  }
}
</script>

<template>
  <section @keyup.enter="fetchWeather()" class="flex flex-col m-5 justify-center items-center">
    <input
      v-model="location"
      :disabled="loading"
      type="text"
      placeholder="Type location here"
      class="border text-2xl"
    />
    <p class="font-bold text-2xl whitespace-pre-line">{{ result }}</p>
  </section>
</template>
