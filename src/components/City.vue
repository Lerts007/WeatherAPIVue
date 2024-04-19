<template>
  <div class="bloc-city" @click="$emit('city', city)">
    <h2>
      {{
        city.name +
        ", " +
        city.country +
        (city.state !== undefined ? ", " + city.state : "")
      }}
    </h2>
  </div>
</template>

<script setup>
const props = defineProps(["city"]);
</script>

<style lang="scss" scoped>
h2 {
  font-size: 3rem;
  text-align: center;
}
.bloc-city {
  width: 100%;
  margin-top: 20px;
  padding: 5px;

  border: 1px solid white;
  border-radius: 5px;

  background-color: $backgoundHeader;
  cursor: pointer;
}
</style>
