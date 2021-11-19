<template>
  <div id="app">
    <UE v-for="UE in UeList" :key="UE.name" :UE="UE"/>
  </div>
</template>

<script>
import coef_config from "@/assets/coef/app5-info.yaml"; // gets modules coef from yaml file
import UE from "./components/UE.vue";

export default {
  name: "App",
  components: { UE },
  data() {
    return {
      UeList: null,
    };
  },
  created() {
    // Read the UE yml in the assets
    console.log(coef_config.ue);
    this.UeList = coef_config.ue;
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
