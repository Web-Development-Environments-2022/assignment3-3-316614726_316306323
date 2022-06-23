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
              <div>Owner: {{ recipe.owner }}</div>
              <div>when deserved: {{ recipe.whenDeserved }}</div>
            </div>
            Ingredients:
            <ul>
              <li v-for="(r, index) in recipe.ingredients" :key="index">
                {{ r.name }}:{{ r.amount }}&nbsp;{{ r.unit }}
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
  name: "FamilyRecipe",
  props: {
    recipe: Object,
  },
  data() {
    return {};
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
