<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview
          v-if="state !== 'family'"
          class="recipePreview"
          :recipe="r"
        />
        <FamilyRecipe v-else :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
import FamilyRecipe from "./FamilyRecipe.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
    FamilyRecipe,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    state: {
      type: String,
      required: true,
    },
    // searchParams: {
    //   type: Object,
    //   required: false,
    // },
    recipesData: {
      type: Array,
    },
  },
  data() {
    return {
      recipes: [],
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        let response;
        switch (this._props.state) {
          case "random":
            response = await this.axios.get(
              this.$root.store.server_domain + "/recipes/getRandomRecipes"
              // "https://test-for-3-2.herokuapp.com/recipes/random"
            );
            break;
          case "lastWatched":
            response = await this.axios.get(
              this.$root.store.server_domain + "/users/lastWatches"
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              // { withCredentials: true }
            );
            break;
          case "search":
            this.recipes.push(...this.recipesData);
            return;
            break;
          case "favorite":
            response = await this.axios.get(
              this.$root.store.server_domain + "/users/favorites"
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              // { withCredentials: true }
            );
            break;
          case "private":
            response = await this.axios.get(
              this.$root.store.server_domain + "/users/personal"
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              // { withCredentials: true }
            );
            console.log(response);
            break;
          case "family":
            response = await this.axios.get(
              this.$root.store.server_domain + "/users/family"
              // "https://test-for-3-2.herokuapp.com/recipes/random"
              // { withCredentials: true }
            );
            console.log(response);
            break;
          default:
            return;
            break;
        }

        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
    loaded() {
      this.isLoaded = true;
      this.isLoading = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
