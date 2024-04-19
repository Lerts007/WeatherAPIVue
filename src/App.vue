<script setup>
import { ref, watch } from "vue";
import Content from "./components/Content.vue";
import Header from "./components/Header.vue";
import City from "./components/City.vue";

const loading = ref(false);
const nameCity = ref("Moscow");
const selectedCity = ref(null);
const data = ref(null);
const error = ref(null);

function setNameCity(name) {
  nameCity.value = name;
  fetchData(nameCity.value);
}
const setCity = (city) => {
  selectedCity.value = city;
};
watch(nameCity, () => {
  selectedCity.value = null;
  nameCity.value = null;
});

const fetchData = async () => {
  loading.value = true;
  try {
    const responce = await fetch(
      `http://api.openweathermap.org/geo/1.0/direct?q=${nameCity.value}&limit=5&appid=2f1fb8213a60d59791f85192c8e2bf2a`
    );
    if (!responce.ok) {
      throw new Error("Ошибка");
    }
    data.value = await responce.json();
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <Header @nameCity="setNameCity" />

  <main>
    <p v-if="loading">Загрузка....</p>
    <p v-if="error" style="font-size: 30px">{{ error }}</p>
    <div class="content">
      <City
        v-if="!selectedCity"
        v-for="city in data"
        :key="city.id"
        :city="city"
        @city="setCity"
      />
      <Content v-else :city="selectedCity" />
    </div>
  </main>
</template>

<style scoped></style>
