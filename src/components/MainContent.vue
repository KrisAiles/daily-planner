<script setup lang="ts">
import ScheduleDisplay from './ScheduleDisplay.vue';
import NotesDisplay from './NotesDisplay.vue';
import ToDoDisplay from './ToDoDisplay.vue';

</script>

<template>
    <div id="main-container">
        <div id="schedule-container">
            <ScheduleDisplay></ScheduleDisplay>
        </div>
        <div id="to-do-notes-container">
            <div id="to-do-container">
                <ToDoDisplay></ToDoDisplay>
            </div>
            <div id="notes-container">
                <NotesDisplay></NotesDisplay>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';
@use '../assets/mixins.scss';

#main-container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    border-bottom: variables.$borders;
    background-color: white;
}

#schedule-container {
    width: 50%;
    min-width: 20.5rem;
    min-height: 114.125rem;
    padding: 1rem;
    text-align: center;
    border-right: variables.$borders;

    @include mixins.mobile {
        border-right: none;
        border-bottom: variables.$borders;
        width: 100%;
    }
}

#to-do-notes-container {
    width: 50%;
    min-width: 20.5rem;

    @include mixins.mobile {
        width: 100%;
    }
}

#to-do-container {
    display: flex;
    width: 100%;
    min-height: 57.0625rem;
    padding: 1rem;
    border-bottom: variables.$borders;
}

#notes-container {
    width: 100%;
    min-height: 57.0625rem;
    padding: 1rem;
}

</style>