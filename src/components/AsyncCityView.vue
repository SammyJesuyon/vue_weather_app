<template>
    <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
        <p>You're currently previewing this city, click on the '+' icon to start tracking this city.</p>
        </div>
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <p class="text-sm mb-12">
                {{
                    new Date().toLocaleDateString("en-US", {
                    weekday: "short",
                    month: "long",
                    day: "2-digit"
                })
                }}
                {{
                    new Date().toLocaleTimeString("en-US", {
                    timeStyle: "short",
                })
                }}
            </p>
            <p class="text-8xl mb-8">
                {{ Math.round(weatherData.temp) }}&deg;
            </p>
            <p class="capitalize">
                {{ weatherData.weather.description }}
            </p>
            <img class="w-[130px] h-auto" :src="`http://weatherbit.io/static/img/icons/${weatherData.weather.icon}.png`" alt="icon">
        </div>
        <hr class="border-white border-opacity-10 border w-full">
        <!--Hourly weather-->
        <div class="max-w-screen-md w-full py-12">
            <div class="mx-8 text-white">
                <h2 class="mb-4">16-day Weather</h2>
                <div class="flex gap-10 overflow-x-scroll">
                    <div class="flex flex-col gap-4 items-center" v-for="day in dailyData" :key="day.datetime">
                        <p class="whitespace-nowrap text-md">
                        {{
                            new Date(day.datetime).toLocaleDateString("en-US", {
                            weekday: "long",
                        })
                        }}
                        </p>
                        <img class="w-auto h-[50px] object-cover" :src="`http://weatherbit.io/static/img/icons/${day.weather.icon}.png`" alt="icon">
                        <p class="text-2xl">{{ Math.round(day.temp) }}&deg;</p>
                    </div>
                </div>
            </div>
        </div>
        <hr class="border-white border-opacity-10 border mb-4 w-full">
        
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();

const options = {
  method: 'GET',
  url: 'https://weatherbit-v1-mashape.p.rapidapi.com/current',
  params: {lon: route.query.lon, lat: route.query.lat},
  headers: {
    'X-RapidAPI-Key': '727cf143c3msh39a2a1ff2b9e9e4p14e27ejsn681c486ef597',
    'X-RapidAPI-Host': 'weatherbit-v1-mashape.p.rapidapi.com'
  }
};
const weatherData = await axios.request(options).then(async function (response) {
	return response.data.data[0];
}).catch(function (error) {
	console.error(error);
});
console.log(weatherData, 'weatherData');

const dailyOptions = {
  method: 'GET',
  url: 'https://weatherbit-v1-mashape.p.rapidapi.com/forecast/daily',
  params: {lat: route.query.lat, lon: route.query.lon},
  headers: {
    'X-RapidAPI-Key': '727cf143c3msh39a2a1ff2b9e9e4p14e27ejsn681c486ef597',
    'X-RapidAPI-Host': 'weatherbit-v1-mashape.p.rapidapi.com'
  }
};
const dailyData = await axios.request(dailyOptions).then(function (response) {
    return response.data.data;
}).catch(function (error) {
	console.error(error);
});
console.log(dailyData, 'dailyData');

const minuteOptions = {
  method: 'GET',
  url: 'https://weatherbit-v1-mashape.p.rapidapi.com/forecast/minutely',
  params: {lat: route.query.lat, lon: route.query.lon},
  headers: {
    'X-RapidAPI-Key': '727cf143c3msh39a2a1ff2b9e9e4p14e27ejsn681c486ef597',
    'X-RapidAPI-Host': 'weatherbit-v1-mashape.p.rapidapi.com'
  }
};

const minuteData = await axios.request(minuteOptions).then(function (response) {
	console.log(response.data);
}).catch(function (error) {
	console.error(error);
});
</script>
