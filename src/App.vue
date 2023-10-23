<template>
  <div id="app">
    <nav class="navbar bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">          
          Gerson Estrada
        </a>
      </div>
    </nav>
    <div class="container">
      <div class="row">
        <div class="col">
          <StateList :states="states"/>
        </div>
        <div class="col">
          <StateList :isSearchable="true" :states="states"/>
        </div>  
      </div>
    </div>    
  </div>
</template>

<script>
  import axios from "axios";
  import StateList from "./components/StateList.vue";

export default {
  name: "App",
  data() {
      return {        
        states: [],
      };
    },
  mounted() {
    this.fetchStates();
  },
  methods: {
    async fetchStates() {
        try {
          const response = await axios.get("/api/state");
          this.states = response.data;
          
        } catch(error) {
          console.error('Error fetching states:', error);
        }
      },
  },
  components: {
    StateList,    
  },
};
</script>

<style>
/* Add Bootstrap styles here */
.navbar {
  margin-bottom: 20px;
}
</style>
