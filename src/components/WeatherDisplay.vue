<script setup lang="ts">
import { ref } from 'vue';

//const currentWeather = ref({location: '', max: 0, min: 0, precip: 0, condition: '', src: ''});
const long = ref<number>();
const lat = ref<number>();
const location = ref('');
const max = ref(0);
const min = ref(0);
const precip = ref(0);
const condition = ref('');
const source = ref('');
const showPostecodeInput = ref(false);
const postcode = ref('');
const showForecast = ref(false);

function getLocation() {
    if (!navigator.geolocation) {
    console.log("Geolocation available");
    navigator.geolocation.getCurrentPosition(showPosition, declineLocation);
    } else {
    console.log("Geolocation is not supported by this browser.");
    showPostecodeInput.value = !showPostecodeInput.value;
    }
}

function showPosition(position) {
    console.log("Latitude: " + position.coords.latitude +
    "<br>Longitude: " + position.coords.longitude);
    long.value = position.coords.longitude;
    lat.value = position.coords.latitude;
    const longLat = `${lat.value},${long.value}`;
    fetchForecast(longLat);
}

function declineLocation() {
    console.log("Geolocation declined");
    showPostecodeInput.value = !showPostecodeInput.value;
}

getLocation();

function handlePostcode() {
    console.log(postcode.value);
    const regex = /^([A-Z]{1,2}[0-9][A-Z0-9]?)\s?[0-9][A-Z]{2}$/i;
    if (regex.test(postcode.value)) {
        console.log('correct postcode');
        postcode.value = postcode.value.split(' ').join('').toUpperCase();
        console.log(postcode.value);
        showPostecodeInput.value = !showPostecodeInput.value;
        fetchForecast(postcode.value);
    } else {
        alert('Please enter a valid uk postcode e.g. KT4 8UT');
        postcode.value = '';
    }

}

async function fetchForecast(userLocation: string) {
    const response = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=3b232715630942be928121139251004&q=${userLocation}&days=1&aqi=no&alerts=no`);
    const json = await response.json();
    console.log(json);
    location.value = json.location.name;
    console.log(location.value);
    max.value = json.forecast.forecastday[0].day.maxtemp_c;
    console.log(max.value);
    min.value = json.forecast.forecastday[0].day.mintemp_c
    console.log(min.value);
    precip.value = json.forecast.forecastday[0].day.totalprecip_mm
    console.log(precip.value);
    condition.value = json.forecast.forecastday[0].day.condition.text;
    console.log(condition.value);
    source.value = `https:${json.forecast.forecastday[0].day.condition.icon}`;
    console.log(source.value);
    showForecast.value = !showForecast.value;
}

//fetchForecast();

/*if (location.value) {
    console.log('true');
} else {
    console.log('false');
}*/

/*if ('geolocation' in navigator) {
  console.log('Geolocation is Available');
} else {
  console.log('Geolocation is NOT Available');
}*/

</script>

<template>
    <div id="weather-container">
        <form @submit.prevent="handlePostcode" v-if="showPostecodeInput">
            <label for="postcode">Enter your postcode for todays forecast</label>
            <input id="postcode" name="postcode" type="text" v-model="postcode"><br>
            <input id="submit" name="submit" type="submit">
        </form>
        <div id="forecast-container" v-if="showForecast">
            <div id="location">
                <h3>{{ location }}</h3>
            </div>
            <div id="forecast">                
                <div id="forecast-details">
                    <p>Max temperature: {{ max }}C</p>
                    <p>Min temperature: {{ min }}C</p>
                    <p>Total rainfall: {{ precip }}mm</p>
                    <p>Conditions: {{ condition }}</p>
                </div>  
                <div id="icon">
                    <img :src="source" :alt="condition" />
                </div>
            </div>
                      
        </div>
    </div>
</template>

<style lang="scss" scoped>
#weather-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 282px;
    height: 141px;
    border-radius: 5px;
    box-shadow: 2px 2px 5px black;
}

#forecast-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    width: 282px;
    height: 141px;
    padding: 8px 0 8px 8px;
}

#location {
    display: flex;
    align-items: flex-start;
    justify-content: center;
    width: 100%;

    h3 {
        font-size: 24px;
        font-weight: 600;
    }
}

#forecast {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
}

#icon {
    width: 64px;
}

#forecast-details {
    width: 100%;
    text-align: left;

    p {
        font-size: 18px;
    }
}
</style>