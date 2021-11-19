<template>
  <div id="app">
    <h1>Polytech trivial teaching unit validation simulator</h1>
    <h2>APP5 Info - 2021 2020 edition</h2>

    <!-- Dispay the total grade -->
    <b-card header="Total grade estimation">
      <b-card-text> {{ totalGrade }} / 20 </b-card-text>
    </b-card>

    <!-- Each module grades -->
    <UE
      v-for="UE in UeList"
      :key="UE.name"
      :UE="UE"
      @gradeUpdate="gradeUpdate"
    />
    <b-card header="Source code">
      <b-card-text>
        Check the Github page :
        <a href="https://github.com/Tomansion/Polytech-coef-simulator">https://github.com/Tomansion/Polytech-coef-simulator</a>
        </b-card-text
      >
    </b-card>
  </div>
</template>

<script>
import coef_config from "@/assets/coef.yaml"; // gets modules coef from yaml file
import UE from "./components/UE.vue";

export default {
  name: "App",
  components: { UE },
  data() {
    return {
      UeList: null,
      totalGrade: 20,
      totalCoef: null,
    };
  },
  created() {
    // Get the UE
    this.UeList = coef_config.ue;

    // Add the total grade to each UE
    this.UeList.forEach((ue) => {
      ue.totalCoef = ue.modules.reduce(
        (total, module) => module.coef + total,
        0
      );
      ue.grade = 20 * ue.totalCoef;
    });

    this.totalCoef = this.UeList.reduce((total, ue) => ue.totalCoef + total, 0);
  },
  methods: {
    gradeUpdate({ UE, grade }) {
      // Update the total grade
      this.UeList.find((ue) => ue.name === UE).grade = grade;

      this.updateTotalGrade();
    },
    updateTotalGrade() {
      // Calculate the total grade
      let totalGrade = 0;
      for (let UE of this.UeList) {
        totalGrade += UE.grade;
      }
      this.totalGrade = (totalGrade / this.totalCoef).toFixed(2);
    },
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
