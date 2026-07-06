<script setup lang="ts">
import { ref, onMounted } from 'vue'

const location = ref('')
const loading = ref(false)
const date = ref(new Date().toLocaleDateString())
const localTime = ref(new Date().toLocaleTimeString('en-GB'))

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
        `${currentForecast}\n` +
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
  <section id="weather" class="flex flex-col items-center justify-center px-10 font-[Poppins]">
    <h3 class="text-center font-bold uppercase my-5 text-2xl sm:text-3xl md:text-4xl lg:text-5xl">
      Current Forecast
    </h3>
    <div class="flex flex-col gap-8 px-10 font-[Poppins] lg:flex-row">
      <div
        v-for="(loc, i) in locations"
        :key="i"
        class="flex flex-col justify-center border w-50 p-3 bg-[#38786a]/95 rounded-2xl text-shadow-md text-shadow-gray-600 text-white hover:scale-105 transition duration-300 ease-in-out"
      >
        <ion-icon
          class="flex items-center justify-center w-full text-3xl drop-shadow-lg"
          :name="iconForecast[rawForecast[loc] ?? ''] ?? 'chevron-collapse'"
        ></ion-icon>
        <p class="text-center whitespace-pre-line">
          {{ results[loc] ?? 'Loading...' }}
        </p>
      </div>
    </div>
  </section>

  <section
    class="relative h-50% w-full overflow-hidden flex flex-col mt-20 md:flex-row font-[Poppins] bg-[#122e21]"
  >
    <video
      autoplay
      muted
      loop
      playsinline
      class="absolute insert-0 w-full h-full object-cover z-0 rotate-y-180"
    >
      <source src="../assets/hero.mp4" type="video/mp4" />
    </video>
    <div
      class="relative flex flex-col items-center text-center md:text-left justify-end w-full text-white h-full mt-10 mb-10 lg:p-20 md:p-10"
    >
      <h2
        class="font-extrabold sm:text-2xl md:text-3xl lg:text-4xl uppercase select-none leading-tight"
      >
        Search your local weather
      </h2>
      <div class="flex items-center w-full justify-center">
        <input
          v-model="location"
          :disabled="loading"
          type="text"
          placeholder="Search your city here"
          class="border lg:text-2xl"
          autocomplete="true"
        />
        <ion-icon
          @click="fetchWeather()"
          class="size-10 cursor-pointer gap-8"
          name="search-circle-outline"
        ></ion-icon>
      </div>
    </div>
    <div
      class="bg-[#4c9e93]/60 relative flex items-center justify-center w-full p-2 text-white text-center md:w-30% sm:text-2xl md:text-3xl lg:text-4xl"
    >
      <p class="whitespace-pre-line">
        <ion-icon
          class="flex items-center justify-center w-full"
          :name="iconForecast[rawForecast[location] ?? ''] ?? 'chevron-collapse'"
        ></ion-icon>
        {{ results[location] ?? 'Your forecast is one search away' }}
      </p>
    </div>
  </section>
</template>
