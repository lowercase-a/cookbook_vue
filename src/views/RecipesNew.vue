<template>
  <div class="root">
    I am the /recipes/new page
    <p>Title: <input type="text" v-model="newRecipeTitle"></p>
    <p>Chef: <input type="text" v-model="newRecipeChef"></p>
    <p>Ingredients: <input type="text" v-model="newRecipeIngredients"></p>
    <p>PrepTime: <input type="text" v-model="newRecipePrepTime"></p>
    <p>Directions: <input type="text" v-model="newRecipeDirections"></p>
    <button v-on:click="makeRecipe()">Make a new recipe</button>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipePrepTime: "",
      newRecipeDirections: ""
    };
  },
  created: function() {},
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
        this.$router.push("/")
      });
    }
  }
};
</script>
