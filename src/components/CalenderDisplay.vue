<script setup lang="ts">
import { ref } from 'vue';
import DateSlot from '../slots/DateSlot.vue';

const dateNow = new Date();
const currentDate = dateNow.getDate();
const currentYear = dateNow.getFullYear();
const currentMonth = dateNow.getMonth();
const isLeapYear = ref(false);
const daysInMonth = ref<string[]>([]);
const daysInWeek = ['M', 'T', 'W', 'T', 'F', 'S', 'S'];

function getDaysInMonth() {
    if (currentYear % 4 === 0) {
        isLeapYear.value = true;
    } else {
        isLeapYear.value = false;
    };

    let getMonthStartDay: string;
    const zeroIndexMonth = currentMonth + 1;
    if (zeroIndexMonth < 10) {
        getMonthStartDay = String(currentYear) + '-0' + String(zeroIndexMonth) + '-01'; 
    } else {
        getMonthStartDay = String(currentYear) + '-' + String(zeroIndexMonth) + '-01';
    }
    const monthStartDay = new Date(getMonthStartDay).getDay();

    switch (monthStartDay) {
        case 0:
            daysInMonth.value.push('', '', '', '', '', '');
            break;
        case 1:
            break;
        case 2:
            daysInMonth.value.push('');
            break;
        case 3:
            daysInMonth.value.push('', '');
            break;
        case 4:
            daysInMonth.value.push('', '', '');
            break;
        case 5:
            daysInMonth.value.push('', '', '', '');
            break;
        case 6:
            daysInMonth.value.push('', '', '', '', '');
            break;
    }

    if (currentMonth === 0 || currentMonth === 2 || currentMonth === 4 || currentMonth === 6 || currentMonth === 7 || currentMonth === 9 || currentMonth === 11) {
        daysInMonth.value.push('1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29', '30', '31');
    } else if (currentMonth === 3 || currentMonth === 5 || currentMonth === 8 || currentMonth === 10) {
        daysInMonth.value.push('1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29', '30');
    } else {
        if (isLeapYear.value) {
            daysInMonth.value.push('1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29');
        } else {
            daysInMonth.value.push('1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28');
        }
    }
}

getDaysInMonth();

function getCurrentMonth() {
    let month = '';
    switch (currentMonth) {
        case 0:
            month = 'JANUARY';      
            break;
        case 1:
            month = 'FEBRUARY';      
            break;
        case 2:
            month = 'MARCH';      
            break;
        case 3:
            month = 'APRIL';      
            break;
        case 4:
            month = 'MAY';      
            break;
        case 5:
            month = 'JUNE';      
            break;
        case 6:
            month = 'JULY';      
            break;
        case 7:
            month = 'AUGUST';
            break;
        case 8:
            month = 'SEPTEMBER';      
            break;
        case 9:
            month = 'OCTOBER';      
            break;
        case 10:
            month = 'NOVEMBER';      
            break;
        case 11:
            month = 'DECEMBER';      
            break;
    }
    return month;
}

</script>

<template>
    <div id="date-container">
        <div id="date-month">
            <h2>{{ getCurrentMonth() }}</h2>
            <div id="date">
                <div v-for="(day, index) in daysInWeek" :key="index">
                    <DateSlot>
                        <span>{{ day }}</span>
                    </DateSlot>
                </div>
                <div class="month-dates" v-for="num in daysInMonth" :key="num" :style="[num === String(currentDate) ? {backgroundColor: 'grey'} : {backgroundColor: 'olive'}]">
                    <DateSlot>
                        <span>{{ num }}</span>
                    </DateSlot>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
#date-container {
    width: 328px;
    height: 328px;
    padding: 23px;
}

#date-month {
    width: 282px;
    height: 282px;
    border-radius: 16px;
    background-color: olive;
    box-shadow: 2px 2px 5px black;
    
    h2 {
        padding: 5px 0;
        color: white;
    }
}

#date {
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
    flex-wrap: wrap;
    box-sizing: border-box;
    width: 282px;
    height: 242px;
    /*border: 1px solid black;*/


    span {
        font-size: 22px;
        color: white;
    }
}

.month-dates {
    border-radius: 100%;
}
</style>