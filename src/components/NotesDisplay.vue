<script setup lang="ts">
import { ref } from 'vue';

const notesEdit = ref(false);
const notes = ref('');

function toggleNotesEdit() {
    notesEdit.value = !notesEdit.value;
}

function noteDelete() {
    notes.value = '';
    notesEdit.value = !notesEdit.value;
}
function noteSubmit() {
    notesEdit.value = !notesEdit.value;
}

</script>

<template>
    <div id="notes">
        <div id="notes-heading">
            <h2>Notes</h2>
        </div>
        <div id="notes-content" @click="toggleNotesEdit">
            {{ notes }}
        </div>
        <div id="notes-edit" v-if="notesEdit">
            <form @submit.prevent="noteSubmit">
                <textarea v-model="notes" cols="100" rows="20"></textarea><br>
                <input type="submit"><br>
                <button @click="noteDelete">Delete</button>
            </form>
        </div>
    </div>
</template>

<style lang="scss" scoped>
#notes {
    position: relative;
    display: flex;
    flex-direction: column;
    padding: 16px;
    width: 100%;
    border: 2px solid black;
    background-color: white;
}

#notes-heading {
    text-align: center;
}

#notes-content {
    text-align: left;
    width: 100%;
    height: 100%;
    background-color: darkred;
}

#notes-edit {
    position: absolute;
    width: 95%;
    height: 90%;
    padding: 16px;
    background-color: coral;
}
</style>