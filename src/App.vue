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
          <StateList />
        </div>
        <div class="col">
          <StateList :isSearchable="true" />
        </div>  
      </div>
    </div>    
  </div>
</template>

<script>
import StateList from "./components/StateList.vue";

export default {
  name: "App",
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
