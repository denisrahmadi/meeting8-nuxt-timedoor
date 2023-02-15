<template>
  <div class="card col-md-3">
    <img
      class="recipes-content__img card-img-top rounded"
      :alt="recipe.recipeTitle"
      :src="recipe.recipeImage"
    />
    <p class="username">{{ recipe.username }}</p>
    <div class="card-body">
      <nuxt-link
        class="card-text fs-5 text"
        tag="h1"
        :to="{ name: 'recipe-recipeId', params: { recipeId: recipe.id } }"
        style="height: 45px; align-item: center"
      >
        {{ recipe.recipeTitle }}
      </nuxt-link>
      <div class="recipes-content__body__review card-footer bg-transparent">
        <img :src="likeImage" alt="Heart" @click="likeClick" />
        <p>{{ likeCount }} likes</p>
        <button
          style="
            border-radius: 25px;
            border: none;
            background-color: red;
            margin-left: 10px; 
          "
          v-show="icon"
          @click="deleteRecipe"
        >
          <i style="color: white" class="fa fa-times" aria-hidden="true"></i>
        </button>
        <button
          style="
            border-radius: 25px;
            border: none;
            background-color: green;
            margin-left: 10px;
          "
          v-show="icon"
          @click="editRecipe"
        >
          <i
            style="color: white"
            class="fa fa-pencil-square-o"
            aria-hidden="true"
          ></i>
        </button>
        <button @click="handleClick">Click</button>
      </div>
    </div>
    <!-- Button trigger modal -->

    <!-- Modal -->
    <div v-show="showModal">
    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title">Modal title</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true" @click="handleClick">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <p>Modal body text goes here.</p>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" @click="handleClick">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
  </div>
</template>
<script>
export default {
  props: ["recipe", "icon"],
  data() {
    return {
      email: [],
      showModal: false
    };
  },
  computed: {
    likeCount() {
      if (this.recipe.dataLikes?.length === 1) {
        if (this.recipe.dataLikes[0] === "null") {
          return 0;
        }
        return 1;
      }
      return this.recipe.dataLikes?.length;
    },
    likeImage() {
      let userEmail = this.$store.getters.userEmail;
      if (!this.recipe.dataLikes.some((item) => item[0] === userEmail)) {
        return "images/heart-black.png";
      }
      return "images/heart-red.png";
    },
  },
  methods: {
    likeClick() {
      console.log("tes");
      if (!this.$store.getters.isAuthenticated) {
        this.$router.push("/user/login");
      }
      const userEmail = this.$store.getters.userEmail;

      if (this.email?.length === 1 && this.email[0] === "null") {
        this.email[0] = userEmail;
      } else {
        const checkLike = this.email?.filter((item) => item === userEmail);
        if (checkLike?.length === 0) {
          this.email.push(userEmail);
        } else {
          if (this.email?.length === 1) {
            this.email[0] = "null";
          } else {
            const userEmailIndex = this.email?.findIndex(
              (item) => item === userEmail
            );
            this.email?.splice(userEmailIndex, 1);
          }
        }
      }
      let { id, ...newRecipe } = this.recipe;
      if (this.recipe.dataLikes[0] === "null") {
        this.$store.dispatch("likeUpdate", {
          recipeId: this.recipe.id,
          newDataRecipe: {
            ...newRecipe,
            dataLikes: [this.email],
          },
        });
      } else {
        if (!this.recipe.dataLikes.some((item) => item[0] === userEmail)) {
          this.$store.dispatch("likeUpdate", {
            recipeId: this.recipe.id,
            newDataRecipe: {
              ...newRecipe,
              dataLikes: [...this.recipe.dataLikes, this.email],
            },
          });
        }
      }
    },

    deleteRecipe() {
      this.$store.dispatch("deleteRecipe", this.recipe.id);
    },

    handleClick(){
      this.showModal = true
    },

    editRecipe(){
      this.$router.push("/recipe/" + this.recipe.id + "/edit");
    }
  },
};
</script>
<style>
.recipes-content__img {
  width: 100%;
  height: 25vh;
}
.username {
  margin-bottom: 0px;
}
</style>
