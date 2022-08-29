<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)"/>
    </div>
    <p v-if="savedCities.length === 0">
    No locations added! To track a location, search in the field above.
    </p>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import CityCard from "./CityCard.vue";
import { useRouter } from "vue-router";

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }
  const requests = [];
  savedCities.value.forEach((city) => {
    requests.push(axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lon}&appid=952f58520a45d0c4d77322b583592973&units=imperial`))
  });
  console.log(requests, 'requests');
  const weatherData = await Promise.all(requests);

  await new Promise((res)=>setTimeout(res, 1000));

  weatherData.forEach((value, index) => {
    savedCities.value[index].weather = value.data;
  }
  );
}
await getCities();

const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: { state: city.state, city: city.city },
        query: { id: city.id, lat: city.coords.lat, lon: city.coords.lon}
    });
}
</script>
