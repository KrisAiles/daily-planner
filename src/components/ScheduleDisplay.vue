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
                if (j < schedule.value.length/* && !schedule.value[j].description*/) {
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
                <div class="appointment" :style="{backgroundColor: item.color ? item.color : 'white', border: item.color ? 'none' : '1px solid black'}">
                    {{ item.description }}
                </div>
                <div class="edit-appointment-button">
                    <button :disabled="clickable" @click="toggleScheduleEdit(index)">Edit</button>
                </div>
                <div class="edit-appointment-container" v-if="item.edit">
                    <form @submit.prevent="onSubmitSchedule(index)">
                        <label for="appointment">{{ item.name }}</label>
                        <input name="appointment" v-model="item.description" type="text" size="100" required><br>
                        <label for="color">Select a color: </label>
                        <select name="color" v-model="item.color" required>
                            <option disabled value="">Please select a color</option>
                            <option value="red">Red</option>
                            <option value="blue">Blue</option>
                            <option value="green">Green</option>
                        </select><br>
                        <select v-model="item.duration" required>
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
                        <input class="submit" name="submit" type="submit">
                        <button class="delete" @click="deleteSchedule(index)">Delete</button>
                    </form>
                    <div class="close" @click="onClose(index)">X</div>
                </div>
            </div>
        </div>
        
    </div>
</template>

<style lang="scss" scoped>
#hourly-schedule-container {
    width: 100%;
    padding: 16px;
    text-align: center;

    h2 {
        font-size: 32px;
        font-weight: 600;
        padding-bottom: 16px;
    }
}

.schedule-container {
    width: 100%;
    border: 1px solid black;
}

.hour-container {
    position: relative;
    display: flex;
    width: 100%;
    min-height: 30px;
}

.time {
    font-size: 18px;
    font-weight: 600;
    width: 60px;     
    min-height: 30px;       
    padding: 5px;
    border: 1px solid black;
}

.appointment {
    width: 100%;
    min-height: 30px;
    padding: 5px;
    text-align: left;
}

.edit-appointment-button {
    width: 60px;
    min-height: 30px;
    padding: 5px;
    border: 1px solid black;
    
    button {
        cursor: pointer;
    }
}

.edit-appointment-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100px;
    z-index: 999;
    padding: 16px;
    text-align: left;
    background-color: lightgrey;
}

.close {
    position: absolute;
    top: 0;
    right: 0;
    padding: 4px;
    border-bottom: 2px solid black;
    border-left: 2px solid black;
    cursor: pointer;
}

.delete {
    cursor: pointer;
}

.submit {
    cursor: pointer;
}
</style>