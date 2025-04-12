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

#main-container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    border-bottom: variables.$borders;
    background-color: white;
}

#schedule-container {
    width: 50%;
    min-width: 382px;
    padding: 16px;
    text-align: center;
    border-right: variables.$borders;
}

#to-do-notes-container {
    width: 50%;
    min-width: 382px;
}

#to-do-container {
    display: flex;
    width: 100%;
    min-height: 50%;
    padding: 16px;
    border-bottom: variables.$borders;
}

#notes-container {
    display: flex;
    width: 100%;
    min-height: 50%;
    padding: 16px;
}

</style>