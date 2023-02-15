<template>
  <div>
    <app-recipe-list :dataRecipes="recipes" :icon="isIcon"></app-recipe-list>
  </div>
</template>
<script>
import RecipeList from "../../components/recipe/RecipeList.vue";
export default {
  data() {
    return {
      isIcon: true
    }
  },
  components: {
    "app-recipe-list": RecipeList,
  },
  middleware: ["check-auth", "auth"],
  computed: {
    recipes() {
      let userId = this.$store.getters.userId;
      let userData = this.$store.getters.recipeData;
      return userData.filter((item) => item.userId === userId);
    },
  },
};
</script>
