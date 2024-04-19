<template>
  <header>
    <div class="content">
      <input
        type="text"
        placeholder="Moscow"
        v-model="nameCity"
        @keyup.enter="$emit('nameCity', nameCity)"
      />
      <button @click="$emit('nameCity', nameCity)">Отправить</button>
    </div>
  </header>
</template>

<script setup>
import { ref } from "vue";

const nameCity = ref("");
</script>

<style lang="scss" scoped>
header {
  height: 50px;
  background-color: $backgoundHeader;
}
.content {
  padding: 10px;
  display: flex;
  justify-content: center;
}
input {
  width: 300px;
  margin-right: 20px;
  border: none;
  border-bottom: 2px solid #475c5c;
  background-color: $backgoundHeader;
  outline: none;
  font-size: 2rem;
  transition: border-bottom 0.3s;

  &:focus {
    border: none;
    border-bottom: 2px solid #01f4f4;
  }
}
button {
  padding: 5px;
  background-color: #475c5c;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 2rem;
  transition: background-color 0.3s;

  &:active {
    background-color: #01f4f4;
  }
}

@media (min-width: 430px) {
  input {
    width: 190px;
  }
}
</style>
