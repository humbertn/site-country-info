<template>
  <div>
    <h2>State List</h2>
    <ul class="list-group">
      <li
        v-for="state in states"
        :key="state.state"
        class="list-group-item"
        @click="showStateDetails(state)"
        @dblclick="highlightState(state)"
        :class="{ 'list-group-item-secondary': state === highlightedState }"
      >
        {{ state.state }}
      </li>
    </ul>
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
    data() {
      return {
        states: [],
        selectedState: null,
        highlightedState: null,
      };
    },
    mounted() {
      this.fetchStates();
    },
    methods: {
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
</style>
