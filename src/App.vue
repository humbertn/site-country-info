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
          <StateList 
            :states="states" 
            :highlightedStates="highlightedStates"
            @highlight-state="handleHighlightState"/>
        </div>
        <div class="col">
          <StateList 
            :isSearchable="true" 
            :states="states" 
            :highlightedStates="highlightedStates"
            @highlight-state="handleHighlightState"/>
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
        highlightedStates: [],
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
      handleHighlightState(state) {
        const index = this.highlightedStates.indexOf(state);

        if (index === -1) {          
          this.highlightedStates.push(state);
        } else {          
          this.highlightedStates.splice(index, 1);
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
