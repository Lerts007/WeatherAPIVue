<template>
  <div class="weather-5days">
    <div v-for="day in arrayWeatherEveryDay">
      <WeatherDay v-if="day[1].temperature.length === 8" :day="day" />
    </div>
  </div>
</template>

<script setup>
import WeatherDay from "./WeatherDay.vue";

const props = defineProps(["dataTemp"]);
const weatherEvery3Hours = {};
let arrayWeatherEveryDay = null;
console.log(props.dataTemp.list);

// сортировка температуры по датам
props.dataTemp.list.forEach((el, index) => {
  const date = el.dt_txt.split(" ")[0];
  if (!weatherEvery3Hours[date]) {
    weatherEvery3Hours[date] = {
      temperature: [],
      weatherIcon: [],
    };
  }
  weatherEvery3Hours[date].temperature.push(el.main.temp);
  weatherEvery3Hours[date].weatherIcon.push(el.weather[0].icon);
  arrayWeatherEveryDay = Object.entries(weatherEvery3Hours);
});
console.log(weatherEvery3Hours);
console.log(arrayWeatherEveryDay);
</script>

<style lang="scss" scoped>
.weather-5days {
  width: 100%;
  margin-top: 20px;

  display: flex;
  gap: 10px;
}
</style>
