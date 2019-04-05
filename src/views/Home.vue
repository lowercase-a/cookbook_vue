<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Title: <input type="text" v-model="newRecipeTitle"></p>
    <p>Chef: <input type="text" v-model="newRecipeChef"></p>
    <p>Ingredients: <input type="text" v-model="newRecipeIngredients"></p>
    <p>PrepTime: <input type="text" v-model="newRecipePrepTime"></p>
    <p>Directions: <input type="text" v-model="newRecipeDirections"></p>
    <button v-on:click="makeRecipe()">Make a new recipe</button>

    <!-- <h1>recipes: {{ recipes }}</h1> -->
    <div v-for="recipe in recipes">
      <p>{{ recipe.title }}</p>
      <p>{{ recipe.chef }}</p>
      <img v-bind:src="recipe.image_url">
      <button v-on:click="toggleInfo(recipe)">Show more info</button>
      <div v-if="recipe === currentRecipe">
        <p>{{ recipe.ingredients }}</p>
        <p>{{ recipe.directions }}</p>
        <p>{{ recipe.prep_time }}</p>
        <p>title: <input type="text" v-model="recipe.title"></p>
        <p>chef: <input type="text" v-model="recipe.chef"></p>
        <p>ingredients: <input type="text" v-model="recipe.ingredients"></p>
        <p>prep_time: <input type="text" v-model="recipe.prep_time"></p>
        <p>image_url: <input type="text" v-model="recipe.image_url"></p>
        <p>directions: <input type="text" v-model="recipe.directions"></p>
        <button v-on:click="updateRecipe(recipe)">Update the recipe</button>
        <button v-on:click="deleteRecipe(recipe)">Delete the recipe</button>
      </div>
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
      message: "Hello to Vue.js!",
      recipes: [],
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
    makeRecipe: function() {
      console.log('making the new recipe');
      // make a request to the api to the create action
      // response = HTTP.get("/api/recipes")
      // recipes = reponse.parse
      var params = {
        input_title: this.newRecipeTitle,
        input_ingredients: this.newRecipeIngredients,
        input_directions: this.newRecipeDirections,
        input_prep_time: this.newRecipePrepTime,
        input_image_url: this.newRecipeImageUrl,
        input_chef: this.newRecipeChef
      }

      axios.post("/api/recipes", params).then(response => {
        console.log(response);
        this.recipes.push(response.data);
      });
    },
    toggleInfo: function(theRecipe) {
      if (this.currentRecipe === theRecipe) {
        // I am seeing the info already
        this.currentRecipe = {};
      } else {
        // we cannot see the info
        this.currentRecipe = theRecipe;
      }
      console.log('toggling info...');
    },
    updateRecipe: function(theRecipe) {
      console.log(theRecipe)
      console.log('updating the recipe...');
      var params = {
        input_title: theRecipe.title,
        input_ingredients: theRecipe.ingredients,
        input_directions: theRecipe.directions,
        input_prep_time: theRecipe.prep_time,
        input_image_url: theRecipe.image_url,
        input_chef: theRecipe.chef
      };

      axios.patch("/api/recipes/" + theRecipe.id, params).then(response => {
        console.log(response);
        theRecipe = response.data;
      })
    },
    deleteRecipe: function(theRecipe) {
      console.log('deleting the recipe...');
      // make an HTTP request using axios to the destroy action of my API
      axios.delete("/api/recipes/" + theRecipe.id).then(response => {
        console.log(response);
        // delete the recipe from the recipes array
        // find the index of that recipe in the array
        var index = this.recipes.indexOf(theRecipe);
        // remove the item based on that index
        this.recipes.splice(index, 1);
      })
    }
  }
};
</script>
