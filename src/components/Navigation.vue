<script setup lang="ts"></script>

<template>
  <header class="relative font-[Poppins]">
    <p class="flex h-10 bg-amber-500 items-center justify-center text-sm font-medium">
      MET Malaysia Weather API
    </p>
  </header>

  <nav
    class="flex items-center justify-between font-[Poppins] pl-25 pr-25 uppercase bg-gray-100 border-b-2 border-slate-300"
  >
    <div>
      <h1 class="relative font-bold cursor-pointer text-[30px] text-shadow-lg">myweather</h1>
    </div>

    <ul class="flex items-center justify-center flex-row space-x-5 m-2 font-semibold">
      <li
        class="p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"
      >
        <a href="">Home</a>
      </li>
      <li class="p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"><a href="">Weather</a></li>
      <li class="p-1.5 hover:bg-gray-700 hover:rounded-[5px] hover:text-white transition-all duration-600"><a href="">FAQ</a></li>
    </ul>
  </nav>
</template>
