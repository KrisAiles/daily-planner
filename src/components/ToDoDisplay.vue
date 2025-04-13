<script setup lang="ts">
import { ref } from 'vue';

const toDoItems = ref<{description: string, id: number, completed: boolean, edit: boolean}[]>([]);
const addOpen = ref(false);
const editOpen = ref(false);
const addInput = ref('');
const id = ref(0)

function addToDoItem() {
    id.value++;
    const newId = id.value;
    const newDescription = addInput.value;
    const addToDo = {description: newDescription, id: newId, completed: false, edit: false};
    toDoItems.value.push(addToDo);
    addInput.value = '';
    addOpen.value = !addOpen.value;
}

function closeToDoAdd() {
    addInput.value = '';
    addOpen.value = !addOpen.value;
}
function toggleAddOpen() {
    addOpen.value = !addOpen.value;
}

function toggleToDoEdit(index: number) {
    toDoItems.value[index].edit = !toDoItems.value[index].edit;
    editOpen.value = !editOpen.value;
}

function onSubmitToDo(index: number) {    
    toDoItems.value[index].edit = !toDoItems.value[index].edit;
    editOpen.value = !editOpen.value;
}

function deleteToDo(index: number) {
    toDoItems.value.splice(index, 1);
    editOpen.value = !editOpen.value;
}

</script>

<template>
    <div id="to-do-display-container">
        <div id="to-do-header">
            <h2>To-Do List</h2>
            <button class="button-style" @click="toggleAddOpen" :disabled="editOpen">Add</button>          
            <div id="to-do-add-container" v-if="addOpen">
                <form @submit.prevent="addToDoItem">
                    <label for="add-description">To-do: </label>
                    <input name="add-description" class="add-description input-style" type="text" v-model="addInput" required /> <input class="button-style" name="submit" id="submit" type="submit" value="Add" /> <button class="button-style" @click.prevent="closeToDoAdd">Cancel</button>
                </form>
            </div>
        </div>
        <div class="to-do-item" v-for="(item, index) in toDoItems" :key="item.id">
            <div class="to-do-check">
                <input type="checkbox" name="completed" class="completed" v-model="item.completed" :disabled="addOpen" />
            </div>
            <div class="to-do-description" :style="{color: item.completed ? 'lightgray' : 'white', textDecorationLine: item.completed ? 'line-through' : 'none'}">
                <p>{{ item.description }}</p>
            </div>
            <div class="to-do-edit">
                <button class="button-style" @click="toggleToDoEdit(index)" :disabled="addOpen">Edit</button>
            </div>
            <div class="to-do-edit-container" v-if="item.edit">
                <form @submit.prevent="onSubmitToDo(index)">
                    <label for="description">To-do: </label>
                    <input name="description" class="edit-description input-style" type="text" v-model="item.description" required />
                    <input name="submit" class="button-style" id="submit" type="submit" value="Save" /> <button class="button-style" @click="deleteToDo(index)">Delete</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';
@use '../assets/mixins.scss';

#to-do-display-container {
    width: 100%;
    background-color: white;
}

#to-do-header{
    @include mixins.flex-center;
    position: relative;
    text-align: center;
    margin-bottom: 1rem;
    font-size: 1.25rem;

    h2 {
        font-size: 2rem;
        font-weight: 600;
        color: variables.$olive-color;
        padding-right: 1rem;
    }
}

#to-do-add-container {
    @include mixins.flex-center;
    position: absolute;
    top: 0;
    left: 0;
    padding: 0.5rem;
    width: 100%;
    min-height: 3.75rem;
    background-color: variables.$blue-color;
    color: white;
    z-index: 800;

    label {
        display: inline-block;
    }

    @include mixins.tablet {
        font-size: 1rem;
    }

    @include mixins.mobile {
        font-size: 1rem;
    }
}

.to-do-item {
    @include mixins.flex-center;
    position: relative;
    width: 100%;
    min-height: 3.75rem;
    box-shadow: variables.$box-shadow;
    margin: 0.5rem 0;
    padding: 0.5rem;
    background-color: olive;
    color: white;
    font-size: 1.25rem;
}

.to-do-check {
    text-align: center;
    width: 3.125rem;
}

.to-do-description {
    width: 100%;
}

.to-do-edit {
    width: 3.125rem;
    text-align: center;
}

.to-do-edit-container {
    @include mixins.flex-center;
    position: absolute;
    top: 0;
    left: 0;
    text-align: center;
    padding: 0.5rem;
    width: 100%;
    min-height: 3.75rem;
    background-color: variables.$blue-color;
    color: white;
    z-index: 750;

    label {
        display: inline-block;
    }

    @include mixins.tablet {
        font-size: 1rem;
    }

    @include mixins.mobile {
        font-size: 1rem;
    }
}

.add-description {
    width: 80%;
}

.edit-description {
    width: 80%;
}

.button-style {
    @include mixins.button-style;
}

.input-style {
    @include mixins.input-style;
}
</style>