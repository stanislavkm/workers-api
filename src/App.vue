<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <input type="text" v-model="message">
  {{message}}
  <div>
    {{ex}}
  </div>
<!--  <button @click="handleClick">Click</button>-->
</template>

<script setup lang="ts">
import Worker from 'worker-loader!./worker'
import { ref } from "vue";

const message = ref();
const ex = ref(0);
const worker = new Worker();

// При выполнении метода ниже, интерфейс пользователя будет заблокирован до его окончания
// const handleClick = () => {
//   for (ex; ex.value <= 50000; ex.value++) {
//     console.log(ex.value);
//   }
// };

worker.onmessage = (e: any) => {
  ex.value = e.data;
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
