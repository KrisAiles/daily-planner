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

function onCloseEdit() {
    notesEdit.value = !notesEdit.value;
}

</script>

<template>
    <div id="notes-heading">
        <h2>Notes</h2>
        <button class="button-style" @click="toggleNotesEdit" :disabled="notesEdit"><span v-if="!notes">Add</span><span v-else>Edit</span></button>
    </div>
    <div id="notes">        
        <div id="notes-content">
            {{ notes }}
        </div>
        <div id="notes-edit" v-if="notesEdit">
            <form @submit.prevent="noteSubmit">
                <textarea class="input-style" v-model="notes" id="note-description"></textarea><br>
                <input class="button-style" name="note-input" id="note-input" type="submit" value="Save"> <button class="button-style" @click="noteDelete">Delete</button>
            </form>
            <div class="close-edit" @click="onCloseEdit()">X</div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';
@use '../assets/mixins.scss';

#notes {
    position: relative;
    padding: 1rem;
    width: 100%;
    min-height: 51.75rem;
    background-color: olive;
    box-shadow: variables.$box-shadow;
}

#notes-heading {
    @include mixins.flex-center;
    position: relative;
    text-align: center;
    margin-bottom: 1rem;

    h2 {
        font-size: 2rem;
        font-weight: 600;
        color: variables.$olive-color;
        padding-right: 1rem;
    }
}

#notes-content {
    text-align: left;
    width: 100%;
    height: 100%;
    color: white;
    font-family: "Indie Flower", cursive;
    font-size: 2rem;
}

#notes-edit {
    position: absolute;
    top: 0;
    left: 0;
    text-align: center;
    width: 100%;
    height: 100%;
    padding: 2.375rem 1rem 1rem 1rem;
    background-color: variables.$blue-color;
    border: 0.125rem solid white;
}

.close-edit {
    position: absolute;
    top: 0;
    right: 0;
    padding: 0.25rem;
    border-bottom: 0.125rem solid white;
    border-left: 0.125rem solid white;
    color: white;
    cursor: pointer;
    width: 2rem;
    height: 2rem;
    text-align: center;
}

#note-description {
    width: 100%;
    height: 46.875rem;
    margin-bottom: 0.25rem;
}

.button-style {
    @include mixins.button-style;

    span {
        font-family: "Quicksand", sans-serif;
        font-size: 0.75rem;
        font-weight: 600;
    }
}

.input-style {
    @include mixins.input-style;
    padding: 1rem;
    font-family: "Indie Flower", cursive;
    font-size: 2rem;
}
</style>