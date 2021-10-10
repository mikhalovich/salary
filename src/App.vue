<template>
  <div id="app">
    <div class="info-wrapper">
      <div class="input-wrapper">
        <p>Зарплата Нетто:</p>
        <input v-model="inputNettoSalary" type="text" />
      </div>
      <div class="input-wrapper">
        <p>Подоходный налог (%):</p>
        <input v-model="incomeTaxPercent" type="text" />
      </div>
      <div class="input-wrapper">
        <p>ФСЗН с работника (%):</p>
        <input v-model="personalPensionPercent" type="text" />
      </div>
      <div class="input-wrapper">
        <p>ФСЗН с работадателя (%):</p>
        <input v-model="pensionPercent" type="text" />
      </div>

      <div class="input-wrapper">
        <p>Белгосстрах (%):</p>
        <input v-model="belGosStrachPercent" type="text" />
      </div>
    </div>
    <div class="htp-wrapper">
      <div class="htp-input-wrapper">
        <p>ПВТ?</p>
        <input v-model="isHTP" type="checkbox" />
      </div>
      <div v-if="isHTP" class="htp-salary-wrapper">
        <p>Средняя зарплата по стране:</p>
        <input v-model="mediumSalary" type="number" />
      </div>
    </div>
    <button @click="countBrutto()" class="count-button">Рассчет</button>
    <div class="summary-wrapper">
      <p>Начисленная зарплата: {{ bruttoSalary }}</p>
      <p>Подоходный: {{ incomeTax }}</p>
      <p>ФСЗН работника: {{ personalPension }}</p>
      <p>ФСЗН работадателя: {{ pension }}</p>
      <p>Белгосстрах: {{ belGosStrach }}</p>
      <p>Себестоимость: {{ costSalary }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      inputNettoSalary: null,
      incomeTaxPercent: 13,
      personalPensionPercent: 1,
      pensionPercent: 34,
      belGosStrachPercent: 0.6,
      mediumSalary: 0,
      bruttoSalary: null,
      personalPension: null,
      incomeTax: null,
      pension: null,
      belGosStrach: null,
      basePercent: 0.17,
      costSalary: null,
      isHTP: false,
    };
  },
  methods: {
    countBrutto() {
      const brutto = Number(
        (
          Number(this.inputNettoSalary) /
          (1 -
            Number(this.incomeTaxPercent) / 100 -
            Number(this.personalPensionPercent) / 100)
        ).toFixed(2)
      );

      this.personalPension = Number(
        (brutto * (Number(this.personalPensionPercent) / 100)).toFixed(2)
      );
      this.incomeTax = Number(
        (brutto * (Number(this.incomeTaxPercent) / 100)).toFixed(2)
      );

      const salaryForCount =
        this.isHTP && brutto > Number(this.mediumSalary)
          ? Number(this.mediumSalary)
          : brutto;

      this.pension = Number(
        (salaryForCount * (Number(this.pensionPercent) / 100)).toFixed(2)
      );
      this.belGosStrach = Number(
        (salaryForCount * (Number(this.belGosStrachPercent) / 100)).toFixed(2)
      );

      this.costSalary = (
        this.pension +
        this.belGosStrach +
        brutto +
        this.personalPension +
        this.incomeTax
      ).toFixed(2);

      this.bruttoSalary = brutto;
      return "";
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 950px;
}
.info-wrapper {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.input-wrapper {
  display: flex;
  width: 31%;
  height: 20px;
  margin-top: 15px;
  justify-content: space-between;
  align-items: center;
  margin-right: 19%;

  input {
    width: 60px;
  }
}

.htp-wrapper {
  display: flex;
  width: 54%;
  justify-content: left;
  margin-top: 25px;
  align-items: center;
}

.htp-input-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-right: 45px;

  input {
    margin-left: 15px;
  }
}

.htp-salary-wrapper {
  display: flex;
  height: 20px;
  align-items: center;
  justify-content: space-between;

  input {
    margin-left: 25px;
    width: 60px;
  }
}

.count-button {
  width: 85px;
  height: 32px;
  margin-top: 20px;
}

.summary-wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-top: 20px;

  p {
    margin: 5px 0;
  }
}
</style>
