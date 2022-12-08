<script setup>
import { useI18n } from 'vue-i18n'
import { useCycleList, useNow } from '@vueuse/core'
import { intervalToDuration } from 'date-fns'
import { computed, watch } from 'vue'

const { t, locale, d, availableLocales } = useI18n();

const { state, next } = useCycleList(availableLocales);
watch(state, newState => {
  locale.value = newState
});

const flags = {
  'en': 'i-twemoji-flag-united-states',
  'pt-PT': 'i-twemoji-flag-portugal',
  'ja-JP': 'i-twemoji-flag-japan',
  'de': 'i-twemoji-flag-germany',

}
// See the README about tricky timezone issues!
// I figured since this is i18n-friendly, we'd wanna
// make sure the timezones were right :-)
const christmasDate = new Date('2022/12/25')
const now = useNow();
const intervalDuration = computed(() => intervalToDuration({ start: now.value, end: christmasDate }))
</script>

<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <!-- Dates - Check out locales/en.json for the key -->
      <!-- Controls - I give you an .icon-button class if you want to use it -->
      <!-- Flags - the current locale -->
      <i18n-t keypath="christmasIsComing" tag="span">
        <template #date>
          <span class="capitalize">
            {{ d(christmasDate, 'long') }}</span>


        </template>
        <template #time>
          <span class="text-red-500 font-semibold">
            {{ t('day', intervalDuration.days) }}
          </span>
        </template>
      </i18n-t>
      <div class="flex items-center justify-between w-50">
        <button @click="next()" class="icon-button">
          <span class="i-carbon-language" />
        </button>

        <div>
          <span :class="flags[locale]" />
          {{ t('language') }}
        </div>
      </div>


    </section>
  </main>
</template>



<style scoped>
.icon-button {
  @apply text-xl w-32px h-32px rounded-full border-1 border-transparent bg-transparent cursor-pointer duration-300 hover: ring-2 hover:border-green-500 hover:ring-green-500 hover:ring-opacity-40 hover:text-green-600;
}
</style>
