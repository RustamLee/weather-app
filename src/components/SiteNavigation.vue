<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav
      class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The Local weather</p>
        </div>
      </RouterLink>
      <div class="flex gap-3 flex-1 justify-end">
        <i
          class="cursor-pointer duration-150 fa-solid fa-circle-info text-xl hover:text-weather-secondary"
          @click="toggleModal"
        ></i>
        <i
          class="fa-solid fa-plus cursor-pointer duration-150 text-xl hover:text-weather-secondary"
        ></i>
      </div>
      <BaseModal :modalActive="modalActive" @close="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">About:</h1>
          <p class="mb-4">
            The Local Weather allows you to check the weather in your area.
          </p>
          <h2 class="text-2xl">How it works:</h2>
          <ol>
            <li>Searsh for your city or town in the search bar.</li>
            <li>Click on the search result to see the weather in your area.</li>
            <li>
              You can also add a new location by clicking the "+" icon. This
              will save the location to your list of saved locations.
            </li>
          </ol>
          <h2 class="text-2xl">Removing a city</h2>
          <p>
            To remove a city from your list of saved locations, click on the "x"
            icon next to the city name at the home page.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink } from "vue-router";
import BaseModal from "./BaseModal.vue";

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
