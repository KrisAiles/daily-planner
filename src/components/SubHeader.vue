<script setup lang="ts">
import CalenderDisplay from './CalenderDisplay.vue';
import TimeWeatherDisplay from './TimeWeatherDisplay.vue';
import QuoteDisplay from './QuoteDisplay.vue';

</script>

<template>
    <div id="date-quote-container">
        <CalenderDisplay></CalenderDisplay>
        <TimeWeatherDisplay></TimeWeatherDisplay>
        <Suspense>
            <QuoteDisplay></QuoteDisplay>
        </Suspense>
        
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';

#date-quote-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    width: 100%;
    text-align: center;
    border-bottom: variables.$borders;
    background-color: white;
}

</style>