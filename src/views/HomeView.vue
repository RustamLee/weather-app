<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(",");
  router.push({
    name: "cityView",
    params: { state: state.replaceAll(" ", ""), city: city },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapBoxSearchResults = ref(null);
const mapBoxAPItoken =
  "pk.eyJ1IjoicnVzdGlrc2FnYWQiLCJhIjoiY20yMTUzNXhrMGRvaTJrb2JkeXdzdTh3NiJ9.fsi4ESc5W6ZGBXK98os-8Q";
const searchError = ref(null);
const getSearchresults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapBoxAPItoken}&types=place&limit=5`
        );
        mapBoxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }
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
        <p v-if="searchError">Error. Something wrong. Please try again</p>
        <p v-if="!searchError && mapBoxSearchResults.length === 0">
          No result, try different term
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapBoxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>
