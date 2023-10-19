<template>
    <div>
      <h2>State Details</h2>
      <p><strong>State Name:</strong> {{ state.state }}</p>
      <p><strong>Overall Population:</strong> {{ state.population }}</p>
      <p><strong>Number of Counties:</strong> {{ state.counties }}</p>
      <!-- <ul>
        <li v-for="county in state.counties" :key="county.name">
          {{ county.name }} - Population: {{ county.population }}
        </li>
      </ul> -->
      <p><strong>Sum of County Populations:</strong> {{ state.counties }}</p>
      <p>Validation: Sum equals State Population</p>
      <button @click="close">Close</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      state: Object,
    },
    methods: {
    //   calculateSum() {
    //     return this.state.counties.reduce(
    //       (sum, county) => sum + county.population,
    //       0
    //     );
    //   },
    //   validatePopulation() {
    //     return this.calculateSum() === this.state.population;
    //   },
      close() {
        this.$emit("close");
      },
    },
  };
  </script>
  