<template>
  <div id="app">
    <!-- <p>Total Tax : {{ tax }}</p> -->
  </div>
</template>

<script>

const tax = [
  { range: 50000000, percentage: 5 }, // 490jt
  { range: 250000000, percentage: 15 }, // 
  { range: 500000000, percentage: 25 },
  { range: "all", percentage: 30 },
];

export default {
  name: 'App',
  data () {
    return {
      tax: 0
    }
  },
  methods: {
    calculateTax(taxableSalary) {
      let remainingTaxableSalary = taxableSalary;
      let totalTax = 0;
      for (let i = 0; i < tax.length; i++) {
        if (tax[i].range === 'all') {
          totalTax += (remainingTaxableSalary * tax[i].percentage) / 100;
          break;
        }

        const baseRange = i > 0 ? tax[i].range - tax[i - 1].range : tax[i].range;
        if (remainingTaxableSalary >= baseRange) {
          totalTax += (baseRange * tax[i].percentage) / 100;
          remainingTaxableSalary -= baseRange
        } else if (remainingTaxableSalary <= baseRange && remainingTaxableSalary > 0) {
          totalTax += (remainingTaxableSalary * tax[i].percentage) / 100;
          remainingTaxableSalary -= baseRange;
        } else {
          break;
        }
      }
      console.log(totalTax)
      // this.tax = totalTax
    }
  },
  created () {
    // please change this value for testing
    this.calculateTax(10000000)
  }
}
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
