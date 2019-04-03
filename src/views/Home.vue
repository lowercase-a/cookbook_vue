<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <h1>recipes: {{ recipes }}</h1> -->
    <div v-for="recipe in recipes">
      <p>{{ recipe.title }}</p>
      <p>{{ recipe.chef }}</p>
      <p>{{ recipe.image_url }}</p>
      <img v-bind:src="recipe.image_url">
      <hr>
    </div>
  </div>
</template>

<style>
  img {
    width: 80px;
  }
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Goodbye to Vue.js!",
      recipes: []
    };
  },
  created: function() {
    // ruby
    // response = HTTP.get("/api/recipes")
    // recipes = response.parse
    // axios.get("/api/recipes").then(function(response) {
    axios.get("/api/recipes").then(response => {
      this.recipes = response.data;
      // console.log(response.data);
    })
  },
  methods: {}
};
</script>
