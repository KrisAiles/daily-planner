<script setup lang="ts">
import { ref } from 'vue';

const hour = ref('');
const minute = ref('');
const amPm = ref('');
const day = ref('');

function updateTime() {  
    const currentTime = new Date;
    const currentHour = currentTime.getHours();
    const currentMinute = currentTime.getMinutes();
    const currentDay = currentTime.getDay();
    if (currentHour < 10) {
        hour.value = '0' + String(currentHour);
    } else {
        hour.value = String(currentHour);
    };
    if (currentMinute < 10) {
        minute.value = '0' + String(currentMinute);
    } else {
        minute.value = String(currentMinute);
    };
    if (currentHour > 11) {
        amPm.value = 'PM';
    } else {
        amPm.value = 'AM';
    };
    if (currentDay === 0) {
        day.value = 'SUNDAY';
    } else if (currentDay === 1) {
        day.value = 'MONDAY';
    } else if (currentDay === 2) {
        day.value = 'TUESDAY';
    } else if (currentDay === 3) {
        day.value = 'WEDNESDAY';
    } else if (currentDay === 4) {
        day.value = 'THURSDAY';
    } else if (currentDay === 5) {
        day.value = 'FRIDAY';
    } else if (currentDay === 6) {
        day.value = 'SATURDAY';
    } else {
        day.value = '';
    };
};

updateTime();

setInterval(updateTime, 1000);

</script>

<template>
    <div id="time-container">
        <div class="time-display">
            <span>{{ hour }}</span>
            <div class="time-overlay"></div>
            <div id="am-pm">
                <span>{{ amPm }}</span>
            </div>
        </div>
        <div class="time-display">
            <span>{{ minute }}</span>
            <div class="time-overlay"></div>
            <div id="day">
                <span>{{ day }}</span>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';

#time-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 282px;
    height: 135px;
    margin-bottom: 6px;
}

.time-display {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 138px;
    height: 135px;
    background-color: variables.$olive-color;
    border-radius: 5px;
    box-shadow: variables.$box-shadow;

    span {
        font-size: 80px;
        font-weight: 700;
        color: white;
    }
}

.time-overlay {
    position: absolute;
    width: 138px;
    height: 70px;
    top: 0;
    left: 0;
    border-bottom: 1px solid white;
}

#am-pm {
    position: absolute;
    bottom: 0;
    left: 0;
    padding: 4px 8px;

    span {
        font-size: 18px;
    }
}

#day {
    position: absolute;
    bottom: 0;
    right: 0;
    padding: 4px 8px;

    span {
        font-size: 18px;
    }
}

</style>