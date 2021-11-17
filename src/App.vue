<template>
  <div id="app">
    <div>
      <b-card title="Card Title" style="max-width: 40rem">
        <b-card-text>
          Some quick example text to build on the card title and make up the
          bulk of the card's content.
        </b-card-text>

        <b-button href="#" variant="primary">Go somewhere</b-button>
      </b-card>
    </div>
  </div>
</template>

<script>
import UE from "@/assets/coef/app5-info.yaml"; // gets modules coef from yaml file

export default {
  name: "App",
  components: {},
  data() {
    return {
      ue: null,
    };
  },
  created() {
    // Read the UE yml in the assets
    console.log(UE);
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
