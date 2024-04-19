<template>
  <div v-if="loading">Загрузка</div>
  <div v-if="dataTemp" class="content weather">
    <div class="weather-temp">
      <h2>
        {{
          (city.local_names && city.local_names.ru
            ? city.local_names.ru
            : city.name) +
          ", " +
          city.country
        }}
      </h2>
      <p>{{ currentTemp > 0 ? "+" + currentTemp : currentTemp }}°С</p>
      <img :src="weather" width="64px" />
    </div>
    <WeatherDays :dataTemp="dataTemp" />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import WeatherDays from "./WeatherDays.vue";

const props = defineProps(["city"]);
const loading = ref(false);
const city = props.city;
let currentTemp;
let weather;
const dataTemp = ref(null);

onMounted(() => {
  fetchTemp();
});

const fetchTemp = async () => {
  loading.value = true;
  try {
    const response = await fetch(
      `http://api.openweathermap.org/data/2.5/forecast?lat=${city.lat}&lon=${city.lon}&units=metric&appid=2f1fb8213a60d59791f85192c8e2bf2a`
    );
    if (!response.ok) {
      throw new Error("Ошибка");
    }
    dataTemp.value = await response.json();
    currentTemp = Math.round(dataTemp.value.list[0].main.temp);
    weather = `https://openweathermap.org/img/wn/${dataTemp.value.list[0].weather[0].icon}@2x.png`;
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};
</script>

<style lang="scss" scoped>
h2 {
  font-size: 3rem;
  text-align: center;
}
.weather {
  height: calc(100vh - 50px);
  margin-top: 20px;

  display: flex;
  flex-direction: column;
  align-items: center;
}
.weather-temp {
  width: 200px;
  height: 150px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  border-radius: 10px;
  background-color: #e9f3f984;
  & p {
    text-align: center;
    font-size: 3rem;
  }
}
</style>
