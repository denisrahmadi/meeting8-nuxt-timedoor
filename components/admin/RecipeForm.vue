<template>
  <!-- <div class="body">
      <main>
        <div class="add-recipe">
          <label>
            <b>IMAGE LINK : </b>
            <input
              v-model="newRecipe.recipeImage"
              type="text"
              placeholder="https://www.image.com/..."
              class="add-recipe__img"
            />
          </label>
          <input
            v-model="newRecipe.recipeTitle"
            type="text"
            placeholder="Title ..."
            class="add-recipe__title"
          />
          <hr />
          <textarea
            v-model="newRecipe.body"
            placeholder="Your Recipe ....."
            class="add-recipe__desc"
          ></textarea>
          <button class="add-recipe__button" @click="addRecipe">
            Add Recipe
          </button>
        </div>
        {{ newRecipe }}
      </main>
    </div> -->
  <form class="container" @submit.prevent="addRecipe">
    <Input labels="Recipe Title" :model="changeRecipe"></Input>
    <Input labels="Recipe Image" :model="changeImage"></Input>
    <TextArea :model="changeDesc"></TextArea>
    <Input
      labels="Ingredients"
      text="add"
      v-for="(item, index) in ingredientCount"
      :key="item"
      :model="changeIngredient"
      :click="addIngredient"
      :count="ingredientCount"
      :index="index"
      :Delete="deleteIngredient"
      :value="newRecipe.ingredients[index]"
    ></Input>
    <Input
      labels="Directions"
      text="add"
      v-for="(item, index) in directionCount"
      :key="item"
      :model="changeDirection"
      :click="addDirection"
      :count="directionCount"
      :index="index"
      :Delete="deleteDirection"
      :value="newRecipe.directions[index]"
    ></Input>
    <Button text="Submit" @click="addRecipe">
      {{ recipes ? "Update" : "Submit" }}
    </Button>
    {{ newRecipe }}
  </form>
</template>
<script>
// import axios from "axios";
import Button from "../../components/newRecipe/Button.vue";
import Input from "../../components/newRecipe/Input.vue";
import TextArea from "../../components/newRecipe/textArea.vue";

export default {
  props: {
    recipes: {
      type: Object,
      required: false,
    },
  },
  data() {
    return {
      newRecipe: this.recipes
        ? { ...this.recipes }
        : {
            recipeImage: "",
            recipeTitle: "",
            ingredients: [],
            directions: [],
            description: "",
          },
      ingredientCount: 1,
      directionCount: 1,
    };
  },
  mounted() {
    if (this.recipes) {
      this.ingredientCount = this.recipes.ingredients.length;
      this.directionCount = this.recipes.directions.length;
    }
  },
  components: {
    Button,
    Input,
    TextArea,
  },
  middleware: ["check-auth", "auth"],
  methods: {
    addRecipe() {
      if (!this.recipes) {
        this.$store
          .dispatch("addRecipe", this.newRecipe)
          .then(() => this.$router.push("/"));
      } else {
        let { id: _, ...newRecipe } = this.newRecipe;
        this.$store
          .dispatch("updateRecipe", { newRecipe, id: this.newRecipe.id })
          .then(() => {
            this.$router.push("/user");
          });
      }
    },
    changeRecipe(text) {
      this.newRecipe.recipeTitle = text.target.value;
    },
    changeImage(text) {
      this.newRecipe.recipeImage = text.target.value;
    },
    changeDesc(text) {
      this.newRecipe.description = text.target.value;
    },
    changeIngredient(text) {
      if (this.newRecipe.ingredients.length <= this.ingredientCount - 1) {
        this.newRecipe.ingredients.push(text.target.value);
      } else {
        this.newRecipe.ingredients[this.ingredientCount - 1] =
          text.target.value;
      }
    },
    changeDirection(text) {
      if (this.newRecipe.directions.length <= this.directionCount - 1) {
        this.newRecipe.directions.push(text.target.value);
      } else {
        this.newRecipe.directions[this.directionsCount - 1] = text.target.value;
      }
    },
    addIngredient() {
      this.ingredientCount++;
      console.log(this.ingredientCount);
    },
    addDirection() {
      this.directionCount++;
    },
    deleteIngredient(index) {
      this.newRecipe.ingredients = this.newRecipe.ingredients.splice(
        index - 1,
        1
      );
      this.ingredientCount--;
    },
    deleteDirection(index) {
      this.newRecipe.direction = this.newRecipe.direction.splice(index - 1, 1);
      this.directionCount--;
    },
  },
};
</script>
<style>
.body {
  margin: 0px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 40px;
  padding-right: 40px;
  background-color: #eef2e6;
}

main {
  margin: 22px;
}

.recipes {
  display: flex;
  flex-wrap: wrap;
}

.header-nav__link {
  margin-left: 20px;
  color: black;
  text-decoration: none;
  font-size: 20px;
}

/* Recipe Content */
.recipes-content {
  background-color: #eef2e6;
  padding: 10px;
  margin: 13px 13px;
}

.recipes-content__body {
  padding: 5px;
}

.recipes-content__img {
  width: 280px;
  height: 210px;
}

.recipes-content__body__review {
  display: flex;
  align-items: center;
}

.recipes-content__body__review img {
  width: 20px;
  height: 20px;
  margin: 5px 10px 5px 0px;
}

.recipes-content__body__review p {
  margin: 0px;
}

.recipes-content__body__review img:hover {
  cursor: pointer;
}

.recipes-content__body__title {
  margin: 5px 0px;
}

/* Add Recipe */
.add-recipe__title,
.add-recipe__desc {
  display: block;
  width: 100%;
  margin: 10px 0px;
  border: none;
}

.add-recipe__title {
  height: 50px;
  font-size: 25px;
}

.add-recipe__desc {
  height: 300px;
}

.add-recipe__img {
  border: none;
  height: 30px;
}

.add-recipe__button {
  color: white;
  background-color: #4b56d2;
  border: none;
  padding: 20px;
  border-radius: 15px;
  font-weight: bold;
}

.add-recipe__button:hover {
  cursor: pointer;
  background-color: #82c3ec;
  color: #4b56d2;
}

/* Detail */
.recipes-detail {
  display: flex;
  justify-content: center;
}

.recipes-detail div {
  width: 600px;
}

.recipes-detail__img {
  width: 400px;
  height: 300px;
  display: block;
  text-align: center;
  margin: 0px auto;
}

.recipes-detail__title {
  text-align: center;
}
</style>
