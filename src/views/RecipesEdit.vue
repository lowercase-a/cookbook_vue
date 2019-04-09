<template>
  <div class="root">
    I am the edit page
    <div v-for="error in errors">
      {{ error }}
    </div>

    <form v-on:submit.prevent="updateRecipe()">
      <p>Title: <input type="text" v-model="recipe.title"></p>
      <p>Chef: <input type="text" v-model="recipe.chef"></p>
      <p>Ingredients: <input type="text" v-model="recipe.ingredients"></p>
      <p>PrepTime: <input type="text" v-model="recipe.prep_time"></p>
      <p>Directions: <input type="text" v-model="recipe.directions"></p>
      <p>Image Url: <input type="text" v-model="recipe.image_url"></p>
      <input type="submit" value="Update recipe">
      <!-- <button>Make a new recipe</button> -->
    </form>
    <button v-on:click="deleteRecipe()">Delete recipe</button>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      recipe: {
        title: "",
        chef: "",
        ingredients: "",
        prep_time: "",
        directions: "",
        image_url: "",
        id: ""
      },
      errors: []
    };
  },
  created: function() {
    axios.get("/api/recipes/" + this.$route.params.id).then(response => {
      console.log(response.data)
      this.recipe = response.data;
    })
  },
  methods: {
    updateRecipe: function() {
      console.log('making the new recipe');
      // make a request to the api to the create action
      // response = HTTP.get("/api/recipes")
      // recipes = reponse.parse
      var params = {
        input_title: this.recipe.title,
        input_ingredients: this.recipe.ingredients,
        input_directions: this.recipe.directions,
        input_prep_time: this.recipe.prep_time,
        input_image_url: this.recipe.image_url,
        input_chef: this.recipe.chef
      }

      console.log(params);

      axios.patch("/api/recipes/" + this.$route.params.id, params).then(response => {
        console.log('things are going well')
        console.log(response);
        this.$router.push("/recipes/" + this.$route.params.id)
      }).catch(error => {
        console.log('things are going poorly')
        console.log(error.response.data.errors)
        this.errors = error.response.data.errors;
      });
    },
    deleteRecipe: function() {
      console.log('deleting the recipe...');
      // make an HTTP request using axios to the destroy action of my API
      axios.delete("/api/recipes/" + this.$route.params.id).then(response => {
        this.$router.push("/");
      })
    }
  }
};
</script>
