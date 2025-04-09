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
#date-quote-container {
    display: flex;
    width: 100%;
    text-align: center;
    border-bottom: 2px solid gray;
    /*background-color: burlywood;*/
}

</style>