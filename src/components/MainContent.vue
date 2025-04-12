<script setup lang="ts">
import ScheduleDisplay from './ScheduleDisplay.vue';
import NotesDisplay from './NotesDisplay.vue';

</script>

<template>
    <div id="main-container">
        <div id="schedule-container">
            <ScheduleDisplay></ScheduleDisplay>
        </div>
        <div id="to-do-notes-container">
            <div id="to-do-container">
                
            </div>
            <div id="notes-container">
                <NotesDisplay></NotesDisplay>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
#main-container {
    display: flex;
    width: 100%;
    border-bottom: 2px solid gray;
}

#schedule-container {
    width: 50%;
    padding: 16px;
    text-align: center;
    border-right: 2px solid gray;
}

#to-do-notes-container {
    width: 50%;
}

#to-do-container {
    width: 100%;
    min-height: 50%;
    padding: 16px;
    border-bottom: 2px solid gray;
    background-color: aqua;
}

#notes-container {
    display: flex;
    width: 100%;
    min-height: 50%;
    padding: 16px;
}

</style>