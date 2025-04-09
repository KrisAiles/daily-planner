<script setup lang="ts">
import { RouterView } from 'vue-router'
import MainHeader from './components/MainHeader.vue';
import SubHeader from './components/SubHeader.vue';

</script>

<template>
  <header>
    <MainHeader></MainHeader>
  </header>
  <section>
    <SubHeader></SubHeader>
  </section>
  <main>
    <RouterView />
  </main>
</template>

<style lang="scss" scoped>

</style>
