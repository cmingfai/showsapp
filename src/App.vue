<script setup>
import {ref} from "vue";
import BreakingBadCardsSuspense from './components/BreakingBadCardsSuspense.vue';
import RickMortyCards from './components/RickMortyCards.vue';
import Hero from "./components/Hero.vue"

const isBreakingBad=ref(false)
</script>

<template>
 <main>
 <Hero :isBreakingBad="isBreakingBad"
        @selectShow="isBreakingBad=!isBreakingBad"/>
  <!-- <BreakingBadCardsSuspense v-if="isBreakingBad"/>
  <RickMortyCards v-else/> -->
  <KeepAlive>
    <Component :is="isBreakingBad? BreakingBadCardsSuspense:RickMortyCards"/>
  </KeepAlive>
 </main>
</template>

