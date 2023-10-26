<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <div>
    При запуске страницы запускается worker, который меняет значени counterValue и не блокирует основной поток.
    <br>
    После этого нужно сбросить значение counterValue и запустить метод handleClick нажав на BlockMainThread.
    <br>
    В момент выполнения метода handleClick взаимодействие со страницей(ввод в инпут) будет недоступно.
  </div>
  <input type="text" v-model="inputValue" />
    <b>input value:</b> {{ inputValue }}
  <div>
    <b>counter value:</b> {{ counterValue }}
  </div>
  <!--  Кнопка ниже для запуска метода, при котором основной поток блокируется-->
  <button @click="handleBlockThread">BlockMainThread</button>
  <!--  Кнопка ниже для сброса значения после выполнения worker-a-->
  <button @click="counterValue = 0">ResetCounterValue</button>
</template>

<script setup lang="ts">
import Worker from "worker-loader!./worker";
import { ref } from "vue";

const inputValue = ref("");
const counterValue = ref(0);
const worker = new Worker();

// При выполнении метода ниже, интерфейс пользователя будет заблокирован до его окончания
const handleBlockThread = () => {
  for (counterValue; counterValue.value <= 50000; counterValue.value++) {
    console.log(counterValue.value);
  }
};

worker.onmessage = (e: any) => {
  counterValue.value = e.data;
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
