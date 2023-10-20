<template>
  <div>
    <h2>State List</h2>    
    <div class="state-list-container">
      <input
      v-if="isSearchable"
      type="text"
      v-model="searchQuery"
      class="form-control mb-3"
      placeholder="Search for a state"
    />
      <ul class="list-group">
        <li
        v-for="state in filteredStates"
          :key="state.state"
          class="list-group-item"
          @dblclick="showStateDetails(state)"
          @click="highlightState(state)"
          :class="{ 'active': state === highlightedState }"
          >
          {{ state.state }}
        </li>
      </ul>
    </div>
    <StateDetails
      v-if="selectedState"
      :state="selectedState"
      @close="selectedState = null"      
    />
  </div>
</template>

<script>
  import axios from "axios";
  import StateDetails from "./StateDetails.vue";
  
  export default {
    props: {
      isSearchable: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        states: [],
        selectedState: null,
        highlightedState: null,
        searchQuery: "", 
      };
    },
    mounted() {
      this.fetchStates();
    },
    computed: {    
      filteredStates() {
        return this.states.length > 0 ? this.states?.filter((state) =>
          state.state.toLowerCase().includes(this.searchQuery.toLowerCase())
        ) : [];
      },
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
      showStateDetails(state) {
        this.selectedState = state;
      },
      highlightState(state) {
        this.highlightedState = state === this.highlightedState ? null : state;
      },
    },
    components: {
      StateDetails,
    },
  };
  </script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
      margin: 40px 0 0;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      display: inline-block;
      margin: 0 10px;
    }
    a {
      color: #42b983;
    }
    .list-group-item:hover {
      cursor:pointer;
    }
    .state-list-container {
      max-height: 85vh;
      overflow: auto;      
    }
</style>
