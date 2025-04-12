<script setup lang="ts">
import TimeDisplay from './TimeDisplay.vue';
import WeatherDisplay from './WeatherDisplay.vue';

</script>

<template>
    <div id="time-weather-container">
        <TimeDisplay></TimeDisplay>
        <WeatherDisplay></WeatherDisplay>
    </div>
</template>

<style lang="scss" scoped>
#time-weather-container {
    flex: 0 1 auto;
    width: 328px;
    height: 328px;
    padding: 23px;
}

</style>