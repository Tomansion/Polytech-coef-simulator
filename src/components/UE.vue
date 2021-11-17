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
    };
  },
  created() {
    this.totalCoef = this.UE.modules.reduce(
      (total, module) => module.coef + total,
      0
    );
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
};
</script>

<style scoped>
.UE {
  margin: 10px;
}
.moduleRep {
  display: flex;
  justify-content: space-between;
}
.module {
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
