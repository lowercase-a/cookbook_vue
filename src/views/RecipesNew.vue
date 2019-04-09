<template>
  <div class="root">
    I am the /recipes/new page
    <div v-for="error in errors">
      {{ error }}
    </div>

    <form v-on:submit.prevent="makeRecipe()">
      <p>Title: <input type="text" v-model="newRecipeTitle"></p>
      <p>Chef: <input type="text" v-model="newRecipeChef"></p>
      <p>Ingredients: <input type="text" v-model="newRecipeIngredients"></p>
      <p>PrepTime: <input type="text" v-model="newRecipePrepTime"></p>
      <p>Directions: <input type="text" v-model="newRecipeDirections"></p>
      <input type="submit" value="Make a new recipe">
      <!-- <button>Make a new recipe</button> -->
    </form>
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
      newRecipeDirections: "",
      errors: []
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
        console.log('things are going well')
        console.log(response);
        this.$router.push("/")
      }).catch(error => {
        console.log('things are going poorly')
        console.log(error.response.data.errors)
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>
