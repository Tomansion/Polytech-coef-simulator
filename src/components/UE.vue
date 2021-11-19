<template>
  <div :class="'UE ' + (pass ? 'pass' : 'fail')">
    <b-card :header="UE.name + ' ' + (pass ? '' : '(fail)')">
      <!-- Modules repartition -->
      <b-card-text class="moduleRep">
        <div
          v-for="module in UE.modules"
          :key="module.name"
          :style="
            'width:' +
            (module.coef / UE.totalCoef) * 100 +
            '%; background-color:' +
            getColorByCoef(module.coef)
          "
          class="module"
        >
          {{ module.name }} : Coef {{ module.coef }} ({{
            Math.round((module.coef / UE.totalCoef) * 100)
          }}%)
        </div>
      </b-card-text>
      <!-- Grades repartition -->
      <b-card-text class="moduleRep">
        <div
          v-for="module in UE.modules"
          :key="module.name"
          :style="
            'width:' +
            (grades[module.name] / 20) * (module.coef / UE.totalCoef) * 100 +
            '%; background-color:' +
            getColorByCoef(module.coef)
          "
          class="module"
        >
          {{ module.name }}
        </div>
        <!-- A red line at the center of the repartition -->
        <span class="redLine"></span>
        <span class="orangeLine"></span>
      </b-card-text>
      <!-- Grades input -->
      <b-card-text style="display: flex">
        My grades :
        <div class="gradeInputs">
          <div
            v-for="module in UE.modules"
            :key="module.name"
            class="moduleGradeInput"
          >
            {{ module.name }} :
            <span style="display: flex; align-items: center">
              <b-form-input
                v-model="grades[module.name]"
                @input="update"
                type="number"
                min="0"
                max="20"
                step="1"
              >
              </b-form-input>
              / 20
            </span>
          </div>
        </div>
        <div class="totalCoef">
          Grade :
          {{ (Math.round((totalGrade / maxGrade) * 200) / 10).toFixed(2) }} / 20
        </div>
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
      maxGrade: null,
      totalGrade: 0,
      grades: {},
    };
  },
  created() {
    this.maxGrade = 20 * this.UE.totalCoef;

    // Set all the grades to 20
    this.UE.modules.map((module) => {
      this.grades[module.name] = 20;
    });

    this.calculateGrade();
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
    update() {
      this.calculateGrade();
      this.$emit("gradeUpdate", {
        UE: this.UE.name,
        grade: this.totalGrade,
      });
    },
    calculateGrade() {
      this.totalGrade = Object.keys(this.grades).reduce(
        (total, moduleName) =>
          parseInt(this.grades[moduleName]) *
            this.UE.modules.find((module) => module.name === moduleName).coef +
          total,
        0
      );
    },
  },
  computed: {
    pass() {
      return this.totalGrade >= this.maxGrade / 2;
    },
  },
};
</script>

<style scoped>
.UE {
  margin: 10px;
  transition: color 1s;
}
.pass {
  color: rgb(0, 68, 90);
}
.UE.fail {
  color: rgb(158, 0, 0);
}
.moduleRep {
  display: flex;
  justify-content: flex-end;
  height: 60px;
}
.redLine {
  position: absolute;
  left: 50%;
  background-color: red;
  width: 3px;
  height: 60px;
}
.orangeLine {
  position: absolute;
  left: 40%;
  background-color: orange;
  width: 3px;
  height: 60px;
}
.module {
  display: flex;
  justify-content: center;
  align-items: center;
  border: solid 1px #ccc;
  overflow: hidden;
  transition: width 0.5s;
}
.totalCoef {
  padding: 5px;
  white-space: nowrap;
  display: flex;
  align-items: center;
}

.gradeInputs {
  flex: 1;
  display: flex;
  justify-content: space-evenly;
}
.moduleGradeInput input {
  width: 70px;
}
</style>
