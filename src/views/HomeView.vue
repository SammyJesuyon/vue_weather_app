<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResult"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        v-if="mapboxSearchResult"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <p class="py-2" v-if="searchError">
          Sorry dude! Something went wrong! Please try again.
        </p>
        <p class="py-2" v-if="!serverError && mapboxSearchResult.length === 0">
          No result match your query dude! Wanna try again?
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResult"
            :key="searchResult.id"
            @click="previewCity(searchResult)"
            class="py-2 cursor-pointer z-10"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback><CityCardSkeleton /></template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import CityList from "../components/CityList.vue";
import CityCardSkeleton from "../components/CityCardSkeleton.vue";

const router = useRouter();
const previewCity = (searchResult) => {
  console.log(searchResult);
  const [city, state] = searchResult.place_name.split(", ");
  router.push({
    name: "cityView",
    params: {state: state.replaceAll(" ", ""), city: city.replaceAll(" ", "")},
    query: {
      lat: searchResult.center[1],
      lon: searchResult.center[0],
      preview: true
    }
  })
}

const mapboxAPIKey =
  "pk.eyJ1Ijoic2FtbXlqZXN1eW9uIiwiYSI6ImNsNzJwMXphZDA1dXkzb3IxbnF6dnV1eHQifQ.erub78QX2nm6rWPtgyt6fg";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResult = ref(null);
const searchError = ref(null);

const getSearchResult = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResult.value = result.data.features;
        console.log(mapboxSearchResult.value);
      } catch {
        searchError.value = true;
      }
      return;
    }
    mapboxSearchResult.value = null;
  }, 300);
};
</script>
