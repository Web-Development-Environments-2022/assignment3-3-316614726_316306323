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
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }} likes</div>
            </div>
            Ingredients:
            <ul>
              <li
                v-for="(r, index) in recipe.ingredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <p v-html="recipe.instructions" />
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
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
      // id: {
      //   type: Number,
      //   required: true,
      // },
      // image: {
      //   type: String,
      //   required: true,
      // },
      // ingredients: {
      //   type: Array,
      //   required: true,
      // },
      // instructions: {
      //   type: String,
      //   required: true,
      // },
      // isFavorite: {
      //   type: Boolean,
      //   required: true,
      // },
      // isGlutenFree: {
      //   type: Boolean,
      //   required: true,
      // },
      // isVegan: {
      //   type: Boolean,
      //   required: true,
      // },
      // isVegetarian: {
      //   type: Boolean,
      //   required: true,
      // },
      // isWatched: {
      //   type: Boolean,
      //   required: true,
      // },
      // popularity: {
      //   type: Number,
      //   required: false,
      //   default() {
      //     return undefined;
      //   },
      // },
      // readyInMinutes: {
      //   type: Number,
      //   required: true,
      // },
      // servings: {
      //   type: Number,
      //   required: true,
      // },
      // title: {
      //   type: String,
      //   required: true,
      // },
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

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

      // let {
      //   image,
      //   ingredients,
      //   instructions,
      //   isFavorite,
      //   isGlutenFree,
      //   isVegan,
      //   isVegetarian,
      //   isWatched,
      //   popularity,
      //   readyInMinutes,
      //   servings,
      //   title,
      // } = response.data;
      this.recipe = response.data;

      // let _instructions = analyzedInstructions
      //   .map((fstep) => {
      //     fstep.steps[0].step = fstep.name + fstep.steps[0].step;
      //     return fstep.steps;
      //   })
      //   .reduce((a, b) => [...a, ...b], []);

      // let _recipe = {
      //   instructions,
      //   _instructions,
      //   analyzedInstructions,
      //   extendedIngredients,
      //   aggregateLikes,
      //   readyInMinutes,
      //   image,
      //   title,
      // };

      // this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
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
