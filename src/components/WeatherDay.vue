<template>
  <div class="weather-day weather-temp">
    <p>{{ dayOfWeekStr }}</p>
    <p>{{ formattedDate }}</p>
    <p>{{ averageTemp > 0 ? "+" + averageTemp : averageTemp }}°C</p>
    <img :src="weather" width="64px" />
  </div>
</template>

<script setup>
const props = defineProps(["day"]);
let weather;
let weatherIcon;
let maxCount = 0;

const averageTemp = Math.round(
  props.day[1].temperature.slice(3, 6).reduce((sum, current) => {
    return sum + current;
  }) / 3
);

const iconCounts = props.day[1].weatherIcon.reduce((counts, icon) => {
  counts[icon] = (counts[icon] || 0) + 1;
  return counts;
}, {});

for (const icon in iconCounts) {
  if (iconCounts[icon] > maxCount) {
    weatherIcon = icon;
    maxCount = iconCounts[icon];
  }
}

weather = `https://openweathermap.org/img/wn/${weatherIcon}@2x.png`;

const date = new Date(props.day[0]);

const options = { day: "numeric", month: "long" };
const formattedDate = date.toLocaleDateString("ru-RU", options);

const dayOfWeek = date.getDay();
const daysOfWeek = ["ВС", "ПН", "ВТ", "СР", "ЧТ", "ПТ", "СБ"];
const dayOfWeekStr = daysOfWeek[dayOfWeek];

console.log(formattedDate);
console.log(dayOfWeekStr);

console.log(averageTemp);
</script>

<style lang="scss">
.weather-temp {
  width: 200px;
  height: 200px;

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
