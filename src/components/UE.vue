<template>
  <div class="UE">
    <b-card :header="UE.name">
      <!-- Modules repartition -->
      <b-card-text class="moduleRep">
        <div
          v-for="module in UE.modules"
          :key="module.name"
          :style="
            'width:' +
            (module.coef / totalCoef) * 100 +
            '%; background-color:' +
            getColorByCoef(module.coef)
          "
          class="module"
        >
          {{ module.name }} : {{ module.coef }} ({{
            Math.round((module.coef / totalCoef) * 100)
          }}%)
        </div>
        <div class="totalCoef">Total coef : {{ totalCoef }}</div>
      </b-card-text>
      <!-- Grades repartition -->
      <b-card-text class="moduleRep" v-if="grades">
        <div
          v-for="module in UE.modules"
          :key="module.name"
          :style="
            'width:' +
            (grades[module.name] / 20) * (module.coef / totalCoef) * 100 +
            '%; background-color:' +
            getColorByCoef(module.coef)
          "
          class="module"
        >
          {{ module.name }} : {{ grades[module.name] }} / 20
        </div>
        <div class="totalCoef">Grade : {{ totalGrade }} / {{ maxGrade }}</div>
      </b-card-text>
      <!-- Grades input -->
      <!-- TODO -->
    </b-card>
  </div>
</template>

<script>
export default {
  name: "UE",
  props: {
    UE: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      totalCoef: null,
      maxGrade: null,
      grades: {},
    };
  },
  created() {
    this.totalCoef = this.UE.modules.reduce(
      (total, module) => module.coef + total,
      0
    );
    this.maxGrade = this.UE.modules.length * 20;

    // Set all the grades to 20
    this.UE.modules.map((module) => {
      this.grades[module.name] = 20;
    });
  },
  methods: {
    getColorByCoef(coef) {
      let colors = [
        "#b4f8c8",
        "#ffd4db",
        "#fbe7c6",
        "#ffaebc",
        "#bbe7fe",
        "#d3b5e5",
        "#a0e7e5",
        "#eff1db",
      ];
      return colors[(coef + 1) % colors.length];
    },
  },
  computed: {
    totalGrade() {
      if (this.grades)
        return Object.values(this.grades).reduce(
          (total, grade) => grade + total,
          0
        );
      return 0;
    },
  },
};
</script>

<style scoped>
.UE {
  margin: 10px;
}
.moduleRep {
  display: flex;
  justify-content: space-between;
  height: 60px;
}
.module {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 1px;
  padding: 5px;
  border: solid 1px #ccc;
}
.totalCoef {
  padding: 5px;
  white-space: nowrap;
  display: flex;
  align-items: center;
}
</style>
