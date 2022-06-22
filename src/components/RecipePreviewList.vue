<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
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
            console.log(response);
            break;
          case "search":
            this.recipes.push(...this.recipesData);
            return;
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
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
