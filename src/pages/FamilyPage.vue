<template>
  <div class="container">
    <h1 class="title">Family Page!</h1>

    <RecipePreviewList
      v-if="$root.store.username"
      title="Family Recipes"
      state="family"
      :class="{
        RandomRecipes: true,
        center: true,
      }"
    ></RecipePreviewList>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";

export default {
  name: "family",
  components: { RecipePreviewList },
  data() {
    return {};
  },
  validations: {},
  methods: {},
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
