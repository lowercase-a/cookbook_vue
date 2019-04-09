<template>
  <div class="root">
    i am the show page
    <p>{{ recipe.title }}</p>
    <p>{{ recipe.chef }}</p>
    <img v-bind:src="recipe.image_url">
    <p>{{ recipe.ingredients }}</p>
    <p>{{ recipe.directions }}</p>
    <p>{{ recipe.prep_time }}</p>
    <router-link v-bind:to="'/recipes/' + recipe.id + '/edit'">Edit the recipe</router-link>
  </div>
</template>




<script>
import axios from "axios"

export default {
  data: function() {
    return {
      recipe: {
        title: "tortoleeney",
        chef: "brian",
        image_url: "https://tastesbetterfromscratch.com/wp-content/uploads/2015/07/Roasted_Red_Pepper_Tortellini1.jpg",
        ingredients: "torts and lini",
        directions: "boil em",
        prep_time: "20"
      }
    };
  },
  created: function() {
    axios.get("/api/recipes/" + this.$route.params.id).then(response => {
      console.log(response.data)
      this.recipe = response.data;
    })
    // console.log(this.$route.params.id)
  },
  methods: {}
};
</script>
