<script setup lang="ts">
import { ref } from 'vue';

const schedule = ref([{name: '06:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '06:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '07:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '07:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '08:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '08:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '09:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '09:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '10:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '10:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '11:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '11:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '12:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '12:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '13:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '13:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '14:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '14:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '15:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '15:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '16:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '16:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '17:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '17:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '18:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '18:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '19:00', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '19:30', description: '', color: '', edit: false, duration: '0', submitted: false}, {name: '20:00', description: '', color: '', edit: false, duration: '0', submitted: false}]);
const clickable = ref(false);
const appointmentClash = ref(false);

function toggleScheduleEdit(index: number) {
    schedule.value[index].edit = !schedule.value[index].edit;
    clickable.value = !clickable.value;
}

function onClose(index: number) {
    if (!schedule.value[index].submitted) {
        schedule.value[index].description = '';
        schedule.value[index].color = '';
        schedule.value[index].duration = '0';
    }
    schedule.value[index].edit = false;
    clickable.value = !clickable.value;
}

function onSubmitSchedule(index: number) {
    const durationIndex = index + Number(schedule.value[index].duration);
    let i = index + 1;
    
    for (let l = i; l <= durationIndex; l++) {
        if (l < schedule.value.length && schedule.value[l].description) {
            appointmentClash.value = !appointmentClash.value;
        };
    };

    if (!appointmentClash.value) {
        if (i < schedule.value.length) {        
            while (i < schedule.value.length && !schedule.value[i].description) {
                schedule.value[i].color = '';
                i++;
            };
            for (let j = index + 1; j <= durationIndex; j++) {
                if (j < schedule.value.length) {
                    schedule.value[j].description = '';
                    schedule.value[j].color = schedule.value[index].color;
                };                
            };
        };    
        schedule.value[index].submitted = !schedule.value[index].submitted;
        clickable.value = !clickable.value;           
        schedule.value[index].edit = !schedule.value[index].edit;
    } else {
        alert('This appointment clashes with an existing appointment.');
        appointmentClash.value = false;
        return;
    };
}

function deleteSchedule(index: number) {
    let i = index + 1;
    if (i < schedule.value.length) {
        while (i < schedule.value.length && !schedule.value[i].description) {
            schedule.value[i].color = '';
            i++;
        }            
    }
    schedule.value[index].submitted = !schedule.value[index].submitted;
    schedule.value[index].description = '';
    schedule.value[index].color = '';
    schedule.value[index].duration = '0';
    clickable.value = !clickable.value;
    schedule.value[index].edit = !schedule.value[index].edit;
}

</script>

<template>
    <div id="hourly-schedule-container">
        <h2>Hourly Schedule</h2>
        <div class="schedule-container">
            <div class="hour-container" v-for="(item, index) in schedule" :key="item.name">
                <div class="time">
                    {{ item.name }}
                </div>
                <div class="appointment" :style="{backgroundColor: item.color ? item.color : '#808000', border: item.color ? 'none' : '1px solid white'}">
                    {{ item.description }}
                </div>
                <div class="edit-appointment-button">
                    <button class="button-style" :disabled="clickable" @click="toggleScheduleEdit(index)"><span v-if="!item.color">Add</span><span v-else>Edit</span></button>
                </div>
                <div class="edit-appointment-container" v-if="item.edit">
                    <form @submit.prevent="onSubmitSchedule(index)">
                        <label for="appointment">{{ item.name + ' ' }}</label>
                        <input class="description-input input-style" name="appointment" v-model="item.description" type="text" required><br>
                        <label for="color">Select a color: </label>
                        <select class="input-style" name="color" v-model="item.color" required>
                            <option disabled value="">Please select a color</option>
                            <option value="#400080">Purple</option>
                            <option value="#004080">Blue</option>
                            <option value="#408000">Green</option>
                            <option value="#800080">Pink</option>
                            <option value="#008080">Aqua</option>
                        </select><br>
                        <label for="duration">Duration: </label>
                        <select class="input-style" name="duration" v-model="item.duration" required>
                            <option disabled value="">Please select duration</option>
                            <option value="0">30 minutes</option>
                            <option value="1">1 hour</option>
                            <option value="2">1 hour 30 minutes</option>
                            <option value="3">2 hours</option>
                            <option value="4">2 hours 30 minutes</option>
                            <option value="5">3 hours</option>
                            <option value="6">3 hours 30 minutes</option>
                            <option value="7">4 hours</option>
                            <option value="8">4 hours 30 minutes</option>
                            <option value="9">5 hours</option>
                            <option value="10">5 hours 30 minutes</option>
                            <option value="11">6 hours</option>
                            <option value="12">6 hours 30 minutes</option>
                            <option value="13">7 hours</option>
                            <option value="14">7 hours 30 minutes</option>
                            <option value="15">8 hours</option>
                            <option value="16">8 hours 30 minutes</option>
                            <option value="17">9 hours</option>
                            <option value="18">9 hours 30 minutes</option>
                            <option value="19">10 hours</option>
                            <option value="20">10 hours 30 minutes</option>
                            <option value="21">11 hours</option>
                            <option value="22">11 hours 30 minutes</option>
                            <option value="23">12 hours</option>
                            <option value="24">12 hours 30 minutes</option>
                            <option value="25">13 hours</option>
                            <option value="26">13 hours 30 minutes</option>
                            <option value="27">14 hours</option>
                            <option value="28">14 hours 30 minutes</option>
                        </select><br>
                        <input class="submit button-style" name="submit" type="submit" value="Submit"> <button class="delete button-style" @click="deleteSchedule(index)">Delete</button>
                    </form>
                    <div class="close" @click="onClose(index)">X</div>
                </div>
            </div>
        </div>
        
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';
@use '../assets/mixins.scss';

#hourly-schedule-container {
    width: 100%;
    text-align: center;

    h2 {
        font-size: 2rem;
        font-weight: 600;
        padding-bottom: 1rem;
        color: variables.$olive-color;
    }
}

.schedule-container {
    width: 100%;
    box-shadow: variables.$box-shadow;
    
}

.hour-container {
    position: relative;
    display: flex;
    width: 100%;
    min-height: 3.75rem;
}

.time {
    @include mixins.flex-center;
    font-size: 1.125rem;
    font-weight: 600;
    color: white;
    background-color: variables.$olive-color;
    width: 5.625rem;     
    min-height: 3.75rem;       
    padding: 0.25rem;
    border: 0.0625rem solid white;
}

.appointment {
    @include mixins.flex-center;
    justify-content: flex-start;
    width: 100%;
    min-height: 3.75rem;
    padding: 0.3125rem;
    text-align: left;
    color: white;
    font-size: 1.25rem;
}

.edit-appointment-button {
    @include mixins.flex-center;
    width: 3.75rem;
    min-height: 3.75rem;
    padding: 0.3125rem;
    border: 0.0625rem solid white;
    background-color: variables.$olive-color;
    
    button {
        cursor: pointer;
    }
}

.edit-appointment-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    min-height: 8.5rem;
    z-index: 999;
    padding: 1rem;
    text-align: left;
    background-color: variables.$blue-color;
    border: 0.0625rem solid white;
    color: white;
}

.close {
    position: absolute;
    top: 0;
    right: 0;
    padding: 0.25rem;
    border-bottom: 0.125rem solid white;
    border-left: 0.125rem solid white;
    cursor: pointer;
    height: 2rem;
    width: 2rem;
    text-align: center;
}

.delete {
    cursor: pointer;
}

.submit {
    cursor: pointer;
}

.description-input {
    width: 76%;
}

.input-style {
    @include mixins.input-style;
    margin-bottom: 0.5rem;
}

.button-style {
    @include mixins.button-style;

    span {
        font-family: "Quicksand", sans-serif;
        font-size: 0.75rem;
        font-weight: 600;
    }
}
</style>