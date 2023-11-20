<template>
  <div style="padding: 8px">
    <h1 class="text-4xl text-left" style="font-weight: bold">
      Social Competencies: Beispielprojekt
    </h1>
    <HelloWorld />
    <hr />
    <Chart />
    <p> © Dominik Dablander 2023 </p>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Chart from ".//components/Chart.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    Chart,
  },
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
