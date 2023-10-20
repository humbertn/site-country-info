<template>
    <!-- Use a Bootstrap modal -->
    <div class="modal fade" :class="{ 'show': state, 'd-block': state }" tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">State Details</h5>
            <button type="button" class="close" @click="close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p><strong>State Name:</strong> {{ state.state }}</p>
            <p><strong>Overall Population:</strong> {{ state.population }}</p>
            <p><strong>Number of Counties:</strong> {{ state.counties}}</p>
            <p><strong>Sum of County Populations:</strong> {{ calculateSum() }}</p>
            <p v-if="validatePopulation()" class="valid">Population quantity matches.</p>
            <p v-if="!validatePopulation()" class="not-valid">Population quantity doesn't match.</p>
            <div class="county-list-container">
                <ul>
                    <li v-for="county in stateCounties" :key="county.name">
                        {{ county.county }} - Population: {{ county.population }}
                    </li>
                </ul>
            </div>            
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="close">Close</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
    import axios from 'axios';

  export default {
    props: {
      state: Object,
    },
    data() {
        return {
        stateCounties: [], 
        };
    },
    watch: {    
        state: {
        handler: 'fetchCounties',
        immediate: true,
    },
  },
    methods: {
        async fetchCounties() {
            if (this.state) {
                try {
                const response = await axios.get(`/api/state/${this.state.state}/county`);
                this.stateCounties = response.data;
                } catch (error) {
                console.error("Error fetching counties:", error.message);
                }
            }
        },
      calculateSum() {
        return this.stateCounties.reduce(
          (sum, county) => sum + county.population,
          0
        );
      },
      validatePopulation() {
        return this.calculateSum() === this.state.population;
      },
      close() {
        this.$emit("close");
      },
    },
  };
  </script>
  
  <style scoped>
  .county-list-container {
    max-height: 200px;
    overflow: auto;
  }
  .valid {
    color: green;
    font-weight: bold;
  }
  .not-valid {
    color: red;
    font-weight: bold;
  }
  </style>
  