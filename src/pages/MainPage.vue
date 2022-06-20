<template>
  <div id="main-container">
    <h1 id="main-title" class="title">Welcome To The Main Page!</h1>
    <RecipePreviewList
      title="Random Recipes"
      class="RandomRecipes center"
      state="random"
    />
    <!-- <router-link v-if="!$root.store.username" to="/login" tag="button"
      >You need to Login first!</router-link
    > -->
    <Login v-if="!$root.store.username" />

    <!-- {{ !$root.store.username }} -->
    <RecipePreviewList
      v-else
      title="Last Viewed Recipes"
      state="lastWatched"
      :class="{
        RandomRecipes: true,
        center: true,
      }"
      disabled
    ></RecipePreviewList>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import Login from "../components/Login";

export default {
  components: {
    RecipePreviewList,
    Login,
  },
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#main-container {
  background-image: url("../assets/main-background.jpg");
  background-size: cover;
  background-height: 100px;
}

* {
  text-align: center;
  color: white;
  text-shadow: 2px 0 0 #000, 0 -1px 0 #000, 0 1px 0 #000, -1px 0 0 #000;
}
h1 {
  font-size: 100px;
}
</style>
