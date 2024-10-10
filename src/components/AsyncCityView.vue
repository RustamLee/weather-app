<template>
  <div class="flex flex-col flex-1 items-center">
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      <p>
        Preview of the weather this place. Click "+" to add to your favorites.
      </p>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&appid=bb8008ea9f56a726c097e4a7e14b2ce6&units=imperial`
    );
    return weatherData.data;
  } catch (error) {
    console.log("error");
  }
};
const weatherData = await getWeatherData();
console.log(weatherData);
</script>
