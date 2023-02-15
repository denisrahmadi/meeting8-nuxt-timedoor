<template>
  <div class="container">
    <div class="row g-5">
      <RecipeItem
        v-for="data in dataRecipes"
        :key="data.id"
        :recipe="data"
        :icon="icon"
      ></RecipeItem>
    </div>
  </div>
</template>
<script>
import RecipeItem from "./RecipeItem.vue";
export default {
  components: {
    RecipeItem,
  },
  props: ["dataRecipes", "icon", "isUser"],
};
</script>
<style>
.recipes {
  display: flex;
  flex-wrap: wrap;
}
</style>
