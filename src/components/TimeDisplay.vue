<script setup lang="ts">
import { ref } from 'vue';

const hour = ref('');
const minute = ref('');
const amPm = ref('');

function updateTime() {  
    const currentTime = new Date;
    const currentHour = currentTime.getHours();
    const currentMinute = currentTime.getMinutes();
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
    if ( currentHour > 11) {
        amPm.value = 'PM';
    } else {
        amPm.value = 'AM';
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
        </div>
    </div>
</template>

<style lang="scss" scoped>
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
    background-color: grey;
    border-radius: 5px;
    box-shadow: 2px 2px 5px black;

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

</style>