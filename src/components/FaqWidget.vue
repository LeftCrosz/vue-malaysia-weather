<script setup lang="ts">
import { ref } from 'vue'
import faq_json from '../data/questions.json'

interface faqItems {
  question: string
  answer: string
  open: boolean
}

const faqList = ref<faqItems[]>(faq_json)

function togglefaq(faq: faqItems) {
  faq.open = !faq.open
}
</script>

<template>
<section class="my-10">
  <h1 class="text-center text-[50px] font-bold">FAQ</h1>
  <div
    v-for="(faq, index) in faqList"
    :key="index"
    v-on:click="togglefaq(faq)"
    class="flex flex-col justify-center items-center select-none cursor-pointer"
  >
    <div
      class="bg-gray-400 text-[20px] font-bold min-w-100 p-3 max-w-100 border-gray-200 border"
      :class="{
        'bg-gray-400 text-black': index % 2 == 0,
        'bg-gray-700 text-zinc-200': index % 2 !== 0,
      }"
    >
      <div class="flex justify-between items-center">
        <p>{{ faq.question }}</p>

        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="size-6"
          :class="{ 'rotate-45': faq.open }"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 9v6m3-3H9m12 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
          />
        </svg>
      </div>
      <p
        class="overflow-hidden transition-all ease-in-out duration-200"
        :class="{ 'max-h-20 opacity-100': faq.open, 'max-h-0 opacity-0': !faq.open }"
      >
        {{ faq.answer }}
      </p>
    </div>
  </div>
</section>

</template>
