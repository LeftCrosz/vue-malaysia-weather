<script setup lang="ts"></script>

<template>
  <section
    class="relative h-50% w-full overflow-hidden flex flex-col md:flex-row font-[Poppins]"
  >
    <video autoplay muted loop playsinline class="absolute insert-0 w-full h-full object-cover z-0">
      <source src="../assets/hero.mp4" type="video/mp4" />
    </video>
    <div
      class="relative flex flex-col items-center md:items-start text-center md:text-left justify-end w-full text-white h-full mt-10 lg:p-40 md:p-10"
    >
      <h2
        class="font-extrabold text-2xl sm:text-3xl md:text-4xl lg:text-5xl uppercase select-none leading-tight"
      >
        Bring Umbrella or Nah?
      </h2>
      <p class="font-semibold text-sm sm:text-base select-none">Malaysia Weather Forecast</p>
    </div>
    <div
      class="relative flex items-center justify-center w-full p-2 sm:p-5 text-white mb-10 md:w-30%"
    >
      <svg
        class="transition duration-800 active:rotate-35 hover:rotate-35"
        width="30%"
        height="auto"
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
