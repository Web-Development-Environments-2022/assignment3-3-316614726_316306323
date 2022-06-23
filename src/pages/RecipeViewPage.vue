<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <b-button
              v-if="!recipe.isFavorite"
              variant="outline-danger"
              @click="addToFavorite"
              >Add to Favorites ❤️</b-button
            >
            <b-button v-else variant="outline-danger" disabled
              >Favorite Recipe ❤️</b-button
            >
            <br />
            <b-button v-if="recipe.isWatched" variant="outline-primary" disabled
              >You've seen this recipe before 👁</b-button
            >
          </div>
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }} likes</div>
            </div>
            Ingredients:
            <ul>
              <li v-for="(r, index) in recipe.ingredients" :key="index">
                {{ r.name }}: {{ r.amount }} {{ r.unit }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <p v-html="recipe.instructions" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    recipeId: Number,
  },
  data() {
    return {
      recipe: {
        type: Object,
      },
    };
  },
  async created() {
    try {
      let response;
      try {
        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          this.$root.store.server_domain +
            `/recipes/getRecipe/${this.$route.params.recipeId}`
        );
        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      this.recipe = response.data;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    async addToFavorite() {
      try {
        let response;
        try {
          let response = await this.axios.post(
            this.$root.store.server_domain + `/users/favorites`,
            {
              recipeId: this.recipe.id,
            }
          );
          if (response.status !== 200) this.$router.replace("/NotFound");
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }
        this.recipe.isFavorite = true;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
