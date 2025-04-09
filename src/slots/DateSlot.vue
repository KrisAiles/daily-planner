<script setup lang="ts">

</script>

<template>
    <div class="date-slot">
        <slot></slot>
    </div>
</template>

<style lang="scss" scoped>
.date-slot {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    /*border: 1px solid black;*/
}
</style>