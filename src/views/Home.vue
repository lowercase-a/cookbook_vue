<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <input type="text" v-model="titleFilter">
    <!-- .where(title: 'licorice') -->
    <div v-for="recipe in filterBy(recipes, titleFilter, 'title', 'ingredients')">
      <p>{{ recipe.title }}</p>
      <p>{{ recipe.chef }}</p>
      <!-- <button v-on:click="toggleInfo(recipe)">Show more info</button> -->

      <router-link v-bind:to="'/recipes/' + recipe.id">See more info</router-link>
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
import Vue2Filters from "vue2-filters";


export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Hello to Vue.js!",
      recipes: [],
      titleFilter: "",
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipePrepTime: "",
      newRecipeDirections: "",
      currentRecipe: {}
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
