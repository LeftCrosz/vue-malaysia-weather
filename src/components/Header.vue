<script setup lang="ts"></script>

<template>
  <section class="flex flex-col md:flex-row font-[Poppins] p-6 sm:p-10 lg:p-20 gap-6 bg-gray-100">
    <div
      class="flex flex-col items-center text-center lg:text-left sm:items-start justify-end w-full md:w-auto lg:pl-40"
    >
      <h2
        class="font-extrabold text-3xl sm:text-4xl md:text-5xl lg:text-6xl uppercase select-none leading-tight"
      >
        Bring Umbrella or Nah?
      </h2>
      <p class="font-semibold text-sm sm:text-base select-none">Malaysia Weather Forecast</p>
    </div>
    <div class="flex items-center justify-center w-full p-2 sm:p-5">
      <svg
        class="transition duration-800 hover:rotate-35"
        width="200"
        height="100%"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M7 19.4C7 20.8359 8.11929 22 9.5 22C10.8807 22 12 20.8359 12 19.4V11M12 11C10.3898 11 8 12 8 12C8 12 6.61017 11 5 11C3.38983 11 2 12 2 12C2 6.47715 6.47715 2 12 2C17.5228 2 22 6.47715 22 12C22 12 20.6102 11 19 11C17.3898 11 16 12 16 12C16 12 13.6102 11 12 11Z"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </div>
  </section>
</template>
