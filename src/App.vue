<template>
  <div class="page">
    <div id="app">
      <div class="info-wrapper">
        <div class="input-wrapper">
          <p>Зарплата Нетто:</p>
          <input v-model="inputNettoSalary" type="text">
        </div>
        <div class="input-wrapper">
          <p>Подоходный налог (%):</p>
          <input v-model="incomeTaxPercent" type="text">
        </div>
        <div class="input-wrapper">
          <p>ФСЗН с работника (%):</p>
          <input v-model="personalPensionPercent" type="text">
        </div>
        <div class="input-wrapper">
          <p>ФСЗН с работадателя (%):</p>
        <input v-model="pensionPercent" type="text"></div>
        <div class="input-wrapper">
          <p>Белгосстрах (%):</p>
          <input v-model="belGosStrachPercent" type="text">
        </div>
      </div>
      <div class="htp-wrapper">
        <div class="htp-input-wrapper">
          <p>ПВТ?</p>
          <input v-model="isHTP" type="checkbox">
        </div>
        <div v-if="isHTP" class="htp-salary-wrapper">
          <p>Средняя зарплата по стране:</p>
          <input v-model="mediumSalary" type="number">
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
  </div>
</template>

<script>

export default {
  name: 'App',
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
      const brutto = Number((Number(this.inputNettoSalary)
        / (1 - (Number(this.incomeTaxPercent) / 100)
        - (Number(this.personalPensionPercent) / 100))).toFixed(2));

      this.personalPension = Number((brutto * (Number(this.personalPensionPercent) / 100))
        .toFixed(2));
      this.incomeTax = Number((brutto * (Number(this.incomeTaxPercent) / 100)).toFixed(2));

      const salaryForCount = this.isHTP && (brutto > Number(this.mediumSalary))
        ? Number(this.mediumSalary) : brutto;

      this.pension = Number((salaryForCount * (Number(this.pensionPercent) / 100))
        .toFixed(2));
      this.belGosStrach = Number((salaryForCount * (Number(this.belGosStrachPercent) / 100))
        .toFixed(2));

      this.costSalary = (this.pension
        + this.belGosStrach
        + brutto).toFixed(2);

      this.bruttoSalary = brutto;
      return '';
    },
  },
};
</script>

<style lang="scss">
.page {
  min-height: 900px;
  padding-top: 50px;
  margin: -8px -8px -8px -8px;
  background-image: url("https://i.ibb.co/nb0xjRg/1625271062-18-kartinkin-com-p-bukhgalteriya-fon-krasivie-foni-21-1.jpg");
  background-size: 100%;
}
#app {
  margin: auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-left: 20px;
  padding-bottom: 10px;
  max-width: 950px;
  border-radius: 10px;
  background: rgb(184,184,184);
  background: linear-gradient(
    90deg, rgba(215,215,209,1) 0%, rgba(238,238,240,1) 53%, rgba(214,221,232,1) 100%
  );
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
    display: inline-flex;
    margin: 5px;
    text-decoration: none;
    position: relative;
    font-size: 15px;
    line-height: 15px;
    padding: 12px 30px;
    color: #FFF;
    font-weight: bold;
    text-transform: uppercase;
    font-family: 'Roboto Condensed', Тahoma, sans-serif;
    background: #337AB7;
    cursor: pointer;
    border: 2px solid #BFE2FF;
    outline: 1px solid;
    outline-color: #337AB7;
    outline-offset: 0px;
    text-shadow: none;
    transition: all 1.5s cubic-bezier(0.19, 1, 0.22, 1);
}
.count-button:hover,
.count-button:active,
.count-button:focus {
    box-shadow: inset 0 0 20px #BFE2FF;
    outline-color: rgba(255, 255, 255, 0);
    outline-offset: 15px;
    color: #FFF;
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

@media (max-width: 768px) {
  .page {
    widows: 100%;
    height: 100%;
    background: rgb(184,184,184);
    background: linear-gradient(
    90deg, rgba(215,215,209,1) 0%, rgba(238,238,240,1) 53%, rgba(214,221,232,1) 100%
  );
    padding-top: 0;
  }
  #app {
    width: 100%;
    height: 100%;
    border-radius: 0;
    padding: 0;
  }
  .info-wrapper {
    display: flex;
    flex-direction: column;
    padding: 0 1rem 0 1rem;
    text-align: start;
  }
  .input-wrapper {
    width: 100%;
    margin: 10px 0 10px 0;
  }
  .htp-wrapper {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  .htp-input-wrapper {
    padding-left: 20px;
  }
  .htp-salary-wrapper {
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
    text-align: start;
    p {
      padding-left: 20px;
    }
    input {
      margin-right: 17px;
    }
  }
  .summary-wrapper {
    p {
    margin: 10px 0 10px 20px;
    }
  }
}
</style>
