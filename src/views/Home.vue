<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Title: <input type="text" v-model="newRecipeTitle"></p>
    <p>Chef: <input type="text" v-model="newRecipeChef"></p>
    <p>Ingredients: <input type="text" v-model="newRecipeIngredients"></p>
    <p>Time: <input type="text" v-model="newRecipePrepTime"></p>
    <p>Directions: <input type="text" v-model="newRecipeDirections"></p>
    <button v-on:click="makeNewRecipe()">Add a new recipe</button>

    <!-- <h1>recipes: {{ recipes }}</h1> -->
    <div v-for="recipe in recipes">
      <p>{{ recipe.title }}</p>
      <p>{{ recipe.chef }}</p>
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
      message: "Hello to Vue.js!",
      recipes: [],
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipePrepTime: "",
      newRecipeDirections: ""
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
    makeNewRecipe: function() {
      console.log('making new recipe...');
      // gather some data
      // console.log('new recipe title');
      // console.log(this.newRecipeTitle);

      var params = {
        input_title: this.newRecipeTitle,
        input_chef: this.newRecipeChef,
        input_ingredients: this.newRecipeIngredients,
        input_prep_time: this.newRecipePrepTime,
        input_directions: this.newRecipeDirections
      }

      console.log(params);
      // send the data to the API
      axios.post("/api/recipes", params).then(response => {
        console.log(response.data);
        this.recipes.push(response.data);
      });
    }
  }
};
</script>
