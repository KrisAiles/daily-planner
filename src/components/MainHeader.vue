<script setup lang="ts">

</script>

<template>
    <div id="header">
        <h1>Daily Planner</h1>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';

#header {
    width: 100%;
    background-color: white;
    text-align: center;
    border-bottom: variables.$borders;

    h1 {
        color: variables.$olive-color;
        font-size: 2rem;
        font-weight: 600;
    }
}
</style>