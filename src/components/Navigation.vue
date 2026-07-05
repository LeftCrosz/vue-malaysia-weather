<script setup lang="ts">
import { ref } from 'vue'
const menu = ref('menu-outline')
const dropdown = ref(false)

function onToggleMenu() {
  menu.value = menu.value === 'menu-outline' ? 'close' : 'menu-outline'
  dropdown.value = dropdown.value === false ? true : false
}
</script>

<template>
  <header class="relative font-[Poppins]">
    <p class="flex h-10 bg-amber-500 items-center justify-center text-sm font-medium">
      MET Malaysia Weather API
    </p>
  </header>

  <nav
    class="flex items-center justify-between font-[Poppins] px-12 uppercase bg-gray border-b-2 border-slate-300 lg:static lg:flex-row lg:justify-between lg:gap-8"
  >
    <div>
      <h1 class="relative font-bold cursor-pointer text-[30px] text-shadow-lg whitespace-nowrap">
        WEATHER APP
      </h1>
    </div>

    <div
      :class="{ '-top-full': !dropdown, 'top-20': dropdown }"
      class="absolute -top-full left-0 max-lg:bg-white w-full flex flex-col gap-8 items-center lg:static lg:flex-row lg:justify-between"
    >
      <ul class="flex flex-col m-2 font-semibold lg:flex-row lg:gap-8">
        <li
          class="w-full p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"
        >
          <a href="">Home</a>
        </li>
        <li
          class="w-full p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"
        >
          <a href="">Weather</a>
        </li>
        <li
          class="w-full p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"
        >
          <a href="">FAQ</a>
        </li>
      </ul>
    </div>

    <div>
      <ion-icon
        @click="onToggleMenu()"
        :name="menu"
        class="text-3xl cursor-pointer lg:hidden"
      ></ion-icon>
    </div>
  </nav>
</template>
