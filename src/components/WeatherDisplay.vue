<script setup lang="ts">
import { ref } from 'vue';

const long = ref<number>();
const lat = ref<number>();
const location = ref('');
const max = ref(0);
const min = ref(0);
const precip = ref(0);
const condition = ref('');
const source = ref('');
const average = ref(0);
const humidity = ref(0);
const chance = ref(0);
const wind = ref(0);
const sunrise = ref('');
const sunset = ref('');
const showPostecodeInput = ref(false);
const postcode = ref('');
const showForecast = ref(false);
const weatherHover = ref(false);

function getLocation() {
    if ("geolocation" in navigator) {
    console.log("Geolocation available");
    navigator.geolocation.getCurrentPosition(showPosition, declineLocation);
    } else {
    console.log("Geolocation is not supported by this browser.");
    showPostecodeInput.value = !showPostecodeInput.value;
    }
}

function showPosition(position: GeolocationPosition) {
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
        postcode.value = postcode.value.split(' ').join('').toUpperCase();
        showPostecodeInput.value = !showPostecodeInput.value;
        fetchForecast(postcode.value);
    } else {
        alert('Please enter a valid uk postcode e.g. KT4 8UT');
        postcode.value = '';
    }

}

async function fetchForecast(userLocation: string) {
    const response = await fetch(`https://api.weatherapi.com/v1/forecast.json?key=3b232715630942be928121139251004&q=${userLocation}&days=1&aqi=no&alerts=no`);
    const json = await response.json();
    console.log(json);
    location.value = json.location.name;
    console.log(location.value);
    max.value = json.forecast.forecastday[0].day.maxtemp_c;
    console.log(max.value);
    min.value = json.forecast.forecastday[0].day.mintemp_c;
    console.log(min.value);
    precip.value = json.forecast.forecastday[0].day.totalprecip_mm;
    console.log(precip.value);
    condition.value = json.forecast.forecastday[0].day.condition.text;
    console.log(condition.value);
    source.value = `https:${json.forecast.forecastday[0].day.condition.icon}`;
    console.log(source.value);
    average.value = json.forecast.forecastday[0].day.avgtemp_c;
    console.log(average.value);
    humidity.value = json.forecast.forecastday[0].day.avghumidity;
    console.log(humidity.value);
    chance.value = json.forecast.forecastday[0].day.daily_chance_of_rain;
    console.log(chance.value);
    wind.value = json.forecast.forecastday[0].day.maxwind_mph;
    console.log(wind.value);
    sunrise.value = json.forecast.forecastday[0].astro.sunrise;
    console.log(sunrise.value);
    sunset.value = json.forecast.forecastday[0].astro.sunset;
    console.log(sunset.value);
    showForecast.value = !showForecast.value;
}

</script>

<template>
    <div id="weather-container" :style="{boxShadow: weatherHover ? 'none' : '2px 2px 5px black'}">
        <form @submit.prevent="handlePostcode" v-if="showPostecodeInput">
            <label id="weather-label" for="postcode">Enter your postcode for todays forecast</label>
            <input class="input-style" id="postcode" name="postcode" type="text" v-model="postcode"><br>
            <input class="button-style" id="submit" name="submit" type="submit">
        </form>
        <div id="forecast-container" v-if="showForecast" @mouseover="weatherHover = true" @click="weatherHover = true" >
            <div class="location">
                <h3>{{ location }}</h3>
            </div>
            <div class="forecast">                
                <div class="forecast-details">
                    <p>Max temperature: {{ max }}C</p>
                    <p>Min temperature: {{ min }}C</p>
                    <p>Total rainfall: {{ precip }}mm</p>
                    <p>Conditions: {{ condition }}</p>
                </div>  
                <div class="icon">
                    <img :src="source" :alt="condition" />
                </div>
            </div>
            <div id="forecast-expand" v-if="weatherHover" @mouseleave="weatherHover = false" @click="weatherHover = false" >
                <div class="location">
                    <h3>{{ location }}</h3>
                </div>
                <div class="forecast">                
                    <div class="forecast-details">
                        <p>Max temperature: {{ max }}C</p>
                        <p>Min temperature: {{ min }}C</p>
                        <p>Total rainfall: {{ precip }}mm</p>
                        <p>Conditions: {{ condition }}</p>
                        <p>Average temperature: {{ average }}C</p>
                        <p>Humidity: {{ humidity }}%</p>
                        <p>Chance of rain: {{ chance }}%</p>
                        <p>Wind speed: {{ wind }}MPH</p>
                        <p>Sunrise: {{ sunrise }}</p>
                        <p>Sunset: {{ sunset }}</p>
                    </div>  
                    <div class="icon">
                        <img :src="source" :alt="condition" />
                    </div>
                </div>
                <div id="credit">
                    <a href="https://www.weatherapi.com/" title="Free Weather API"><img src='https://cdn.weatherapi.com/v4/images/weatherapi_logo.png' alt="Weather data by WeatherAPI.com"></a>
                </div>
            </div>     
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../assets/mixins.scss';
@use '../assets/variables.scss';

#weather-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 17.625rem;
    height: 8.8125rem;
    border-radius: 0.3125rem;
    box-shadow: variables.$box-shadow;
    background-color: variables.$olive-color;
    color: white;
}

#forecast-container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    width: 17.625rem;
    height: 8.8125rem;
    padding: 0.5rem 0 0.5rem 0.5rem;
}

#forecast-expand {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    width: 17.625rem;
    height: 20.5rem;
    padding: 0.5rem 0 0.5rem 0.5rem;
    background-color: variables.$olive-color;
    box-shadow: variables.$box-shadow;
    border-radius: 0.3125rem;
    z-index: 990;
}

.location {
    display: flex;
    align-items: flex-start;
    justify-content: center;
    width: 100%;
    margin-bottom: 0.5rem;

    h3 {
        font-size: 1.125rem;
        font-weight: 600;
    }
}

.forecast {
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
    width: 100%;
}

.icon {
    width: 4rem;
}

.forecast-details {
    width: 100%;
    text-align: left;

    p {
        font-size: 1rem;
    }
}

#credit {
    width: 100%;
    text-align: center;
    padding: 0.5rem;
}

.button-style {
    @include mixins.button-style;
    margin-top: 0.5rem;
}

.input-style {
    @include mixins.input-style;
}

#postcode {
    margin-top: 0.5rem;
}

#weather-label {
    font-size: 0.875rem;
    font-weight: 600;
}
</style>