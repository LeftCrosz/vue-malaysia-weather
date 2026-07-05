<script setup lang="ts">
import { ref, onMounted } from 'vue'

const location = ref('')
const loading = ref(false)
const date = ref(new Date().toLocaleDateString())

const locations = ref(['Kuala Lumpur', 'Cheras', 'Kajang', 'Cyberjaya'])

const results = ref<Record<string, string>>({})

async function fetchWeatherFor(locationName: string) {
  results.value[locationName] = 'Loading...'
  try {
    const res = await fetch(
      `https://api.data.gov.my/weather/forecast?contains=${locationName}@location__location_name&${date.value}@date&limit=1`,
    )
    const data = await res.json()
    if (data.length > 0) {
      const forecast = data[0]
      results.value[locationName] =
        `${forecast.location.location_name}\nMin Temp: ${forecast.min_temp}°\nMax Temp ${forecast.max_temp}°`
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
  <section class="flex flex-row justify-center gap-8 my-10 px-10 font-[Poppins]">
    <div
      v-for="(loc, i) in locations"
      :key="i"
      class="flex justify-center border w-50 p-3 bg-gray-700 text-white hover:scale-115 transition duration-300 ease-in-out"
    >
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
