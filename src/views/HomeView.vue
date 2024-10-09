<script setup>
import axios from "axios";
import { ref } from "vue";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapBoxSearchResults = ref(null);
const mapBoxAPItoken =
  "pk.eyJ1IjoicnVzdGlrc2FnYWQiLCJhIjoiY20yMTUzNXhrMGRvaTJrb2JkeXdzdTh3NiJ9.fsi4ESc5W6ZGBXK98os-8Q";
const getSearchresults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapBoxAPItoken}&types=place&limit=5`
      );
      mapBoxSearchResults.value = result.data.features;
      console.log(mapBoxSearchResults.value);
      return;
    }
    mapBoxSearchResults.value = null;
  }, 300);
};
</script>
<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        @input="getSearchresults"
        type="text"
        placeholder="Search for location"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
        v-model="searchQuery"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px01 top-[66px]"
        v-if="mapBoxSearchResults"
      >
        <li
          v-for="searchResult in mapBoxSearchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer"
        >
          {{ searchResult.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>
