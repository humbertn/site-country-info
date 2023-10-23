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
          @click="showStateDetails(state)"
          @dblclick="highlightState(state)"
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
  import StateDetails from "./StateDetails.vue";
  
  export default {
    props: {
      isSearchable: {
        type: Boolean,
        default: false,
      },
      states: {
        type: Array,
        default() {
          return []
        },
      }      
    },
    data() {
      return {        
        selectedState: null,
        highlightedState: null,
        searchQuery: "",
        singleClickTimeout: null,
        doubleClickFlag: false,
      };
    },
    computed: {    
      filteredStates() {
        return this.states.length > 0 ? this.states?.filter((state) =>
          state.state.toLowerCase().includes(this.searchQuery.toLowerCase())
        ) : [];
      },
    },
    methods: {
      showStateDetails(state) {
        if (!this.doubleClickFlag) {
          this.singleClickTimeout = setTimeout(() => {
            if (!this.doubleClickFlag) {
              this.selectedState = state;
            }
          }, 300);
        }
      },
      highlightState(state) {
        this.doubleClickFlag = true;
        this.singleClickTimeout = null,
        this.highlightedState = state === this.highlightedState ? null : state;
        setTimeout(() => {
          this.doubleClickFlag = false;
        }, 400);
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
