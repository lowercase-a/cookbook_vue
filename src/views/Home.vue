<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <input type="text" v-model="titleFilter" list="titles">
    <datalist id="titles">
      <option v-for="recipe in recipes">{{ recipe.title }}</option>
    </datalist>

    <button v-on:click="setSortAttribute('title')">Sort by title</button>
    <button v-on:click="setSortAttribute('chef')">Sort by Chef</button>
    <button v-on:click="setSortAttribute('prep_time')">Sort by Prep Time</button>
    <!-- .where(title: 'licorice') -->
    <!-- <div v-for="recipe in filterBy(recipes, titleFilter, 'title', 'ingredients')"> -->
      <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
        <div v-for="recipe in orderBy(recipes, sortAttribute, sortAsc)" v-bind:key="recipe.id">
          <p>{{ recipe.title }}</p>
          <p>{{ recipe.chef }}</p>
          <p>{{ recipe.prep_time }}</p>
          <button v-on:click="removeRecipe(recipe)">Remove Recipe</button>
          <router-link v-bind:to="'/recipes/' + recipe.id">See more info</router-link>
          <hr>
        </div>
      </transition-group>
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
      sortAttribute: "title",
      titleFilter: "",
      sortAsc: 1,
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
  methods: {
    removeRecipe: function(recipe) {
      var index = this.recipes.indexOf(recipe);
      this.recipes.splice(index, 2);
    },
    setSortAttribute: function(attribute) {
      console.log(attribute);
      this.sortAttribute = attribute;
      if (this.sortAsc === 1) {
        this.sortAsc = -1;
      } else {
        this.sortAsc = 1;
      }
    }
  }
};
</script>
