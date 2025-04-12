<script setup lang="ts">
import { ref } from 'vue';

const toDoItems = ref<{description: string, id: number, completed: boolean, edit: boolean}[]>([/*{description: 'this is a to do item', id: 1234, completed: false, edit: false}, {description: 'this is another to do item', id: 4321, completed: false, edit: false}*/]);
const addOpen = ref(false);
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
}

function onSubmitToDo(index: number) {    
    toDoItems.value[index].edit = !toDoItems.value[index].edit;
}

function deleteToDo(index: number) {
    toDoItems.value.splice(index, 1);
}

</script>

<template>
    <div id="to-do-display-container">
        <div id="to-do-header">
            <h2>To-Do List</h2>
            <button @click="toggleAddOpen">Add</button>
            <div id="to-do-add-container" v-if="addOpen">
                <form @submit.prevent="addToDoItem">
                    <label for="add-description">To-do: </label>
                    <input name="add-description" id="add-description" type="text" v-model="addInput" size="60" required /> <input name="submit" id="submit" type="submit" value="Add" /> <button @click.prevent="closeToDoAdd">Cancel</button>
                </form>
            </div>
        </div>
        <div class="to-do-item" v-for="(item, index) in toDoItems" :key="item.id">
            <div class="to-do-check">
                <input type="checkbox" name="completed" id="completed" v-model="item.completed" />
            </div>
            <div class="to-do-description" :style="{color: item.completed ? 'lightgray' : 'white', textDecorationLine: item.completed ? 'line-through' : 'none'}">
                <p>{{ item.description }}</p>
            </div>
            <div class="to-do-edit">
                <button @click="toggleToDoEdit(index)">Edit</button>
            </div>
            <div class="to-do-edit-container" v-if="item.edit">
                <form @submit.prevent="onSubmitToDo(index)">
                    <label for="description">To-do: </label>
                    <input name="description" id="description" type="text" v-model="item.description" size="50" required /> <input name="submit" id="submit" type="submit" value="Save" /> <button @click="deleteToDo(index)">Delete</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';

#to-do-display-container {
    width: 100%;
    background-color: white;
}

#to-do-header{
    position: relative;
    text-align: center;
}

#to-do-add-container {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 8px;
    width: 99%;
    height: 50px;
    background-color: variables.$blue-color;
    color: white;
}

.to-do-item {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    /*border: 2px solid black;*/
    box-shadow: 2px 2px 5px black;
    margin: 8px 0;
    padding: 8px;
    background-color: olive;
    color: white;
}

.to-do-check {
    text-align: center;
    width: 50px;
}

.to-do-description {
    width: 100%;
}

.to-do-edit {
    width: 50px;
    text-align: center;
}

.to-do-edit-container {
    position: absolute;
    display: flex;
    top: 0;
    left: 0;
    align-items: center;
    justify-content: space-between;
    padding: 8px;
    width: 100%;
    background-color: variables.$blue-color;
    color: white;
}
</style>