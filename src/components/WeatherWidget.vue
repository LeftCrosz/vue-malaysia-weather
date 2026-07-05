<script setup lang="ts">
import { ref, onMounted } from 'vue'

const location = ref('')
const loading = ref(false)
const date = ref(new Date().toLocaleDateString())
const localTime = ref(new Date().toLocaleTimeString('en-GB'))
console.log(localTime)

const locations = ref(['Kuala Lumpur', 'Cheras', 'Kajang', 'Cyberjaya'])

const results = ref<Record<string, string>>({})

const forecastTranslation = ref<Record<string, string>>({
  Berjerebu: 'Hazy',
  'Tiada hujan': 'No rain',
  Hujan: 'Rain',
  'Hujan di beberapa tempat': 'Scattered rain',
  'Hujan di satu dua tempat': 'Isolated rain',
  'Hujan di satu dua tempat di kawasan pantai': 'Isolated rain over coastal areas',
  'Hujan di satu dua tempat di kawasan pedalaman': 'Isolated rain over inland areas',
  'Ribut petir': 'Thunderstorms',
  'Ribut petir di beberapa tempat': 'Scattered thunderstorms',
  'Ribut petir di beberapa tempat di kawasan pedalaman':
    'Scattered thunderstorms over inland areas',
  'Ribut petir di satu dua tempat': 'Isolated thunderstorms',
  'Ribut petir di satu dua tempat di kawasan pantai': 'Isolated thunderstorms over coastal areas',
  'Ribut petir di satu dua tempat di kawasan pedalaman': 'Isolated thunderstorms over inland areas',
  Mendung: 'Overcast',
})

const rawForecast = ref<Record<string, string>>({})
const iconForecast = ref<Record<string, string>>({
  Berjerebu: 'cloud',
  'Tiada hujan': 'cloudy',
  Mendung: 'cloudy',
  Hujan: 'Rain',
  'Hujan di beberapa tempat': 'rainy',
  'Hujan di satu dua tempat': 'rainy',
  'Hujan di satu dua tempat di kawasan pantai': 'rainy',
  'Hujan di satu dua tempat di kawasan pedalaman': 'rainy',
  'Ribut petir': 'thunderstorm',
  'Ribut petir di beberapa tempat': 'thunderstorm',
  'Ribut petir di beberapa tempat di kawasan pedalaman': 'thunderstorm',
  'Ribut petir di satu dua tempat': 'thunderstorm',
  'Ribut petir di satu dua tempat di kawasan pantai': 'thunderstorm',
  'Ribut petir di satu dua tempat di kawasan pedalaman': 'thunderstorm',
})

function translateForecast(value: string) {
  return forecastTranslation.value[value] ?? value
}

async function fetchWeatherFor(locationName: string) {
  results.value[locationName] = 'Loading...'
  try {
    const res = await fetch(
      `https://api.data.gov.my/weather/forecast?contains=${locationName}@location__location_name&${date.value}@date&limit=1`,
    )
    const data = await res.json()
    if (data.length > 0) {
      const forecast = data[0]
      let periodForecast: string

      if (localTime.value <= '11:59:00') {
        periodForecast = forecast.morning_forecast
      } else if (localTime.value <= '17:59:00') {
        periodForecast = forecast.afternoon_forecast
      } else {
        periodForecast = forecast.night_forecast
      }
      rawForecast.value[locationName] = periodForecast

      const currentForecast = translateForecast(periodForecast)

      results.value[locationName] =
        `${forecast.location.location_name}\n` +
        `Current: ${currentForecast}\n` +
        `Low: ${forecast.min_temp}°\n` +
        `High: ${forecast.max_temp}°`
    } else {
      results.value[locationName] = 'No forescast info found.'
    }
  } catch (error) {
    results.value[locationName] = 'Error: ' + error
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  locations.value.forEach((loc) => fetchWeatherFor(loc))
})

async function fetchWeather() {
  if (location.value.trim() === '') return
  loading.value = true
  await fetchWeatherFor(location.value)
  loading.value = false
}
</script>

<template>
  <h3 class="text-center text-[40px] font-bold uppercase my-5">Current Forecast</h3>
  <section class="flex flex-col items-center justify-center gap-8 px-10 font-[Poppins] lg:flex-row">
    <div
      v-for="(loc, i) in locations"
      :key="i"
      class="flex flex-col justify-center border w-50 p-3 bg-green-500 text-white hover:scale-115 transition duration-300 ease-in-out"
    >
      <ion-icon
        class="flex items-center justify-center w-full text-3xl"
        :name="iconForecast[rawForecast[loc] ?? ''] ?? 'chevron-collapse'"
      ></ion-icon>
      <p class="text-center whitespace-pre-line">
        {{ results[loc] ?? 'Loading...' }}
      </p>
    </div>
  </section>

  <section @keyup.enter="fetchWeather()" class="flex flex-col m-5 justify-center items-center">
    <div>
      <input
        v-model="location"
        :disabled="loading"
        type="text"
        placeholder="Search location here"
        class="border text-2xl"
      />
      <ion-icon
        @click="fetchWeather()"
        class="text-3xl cursor-pointer gap-8"
        name="search-circle-outline"
      ></ion-icon>
    </div>

    <p class="font-bold text-2xl whitespace-pre-line">{{ results[location] }}</p>
  </section>
</template>
