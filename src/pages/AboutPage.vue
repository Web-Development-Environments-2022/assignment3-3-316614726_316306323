<template>
  <div class="container">
    <h1>Our Story</h1>
    <p>
      🐶 Pesto & Rachel story began as a joke between two friends at the dogs
      garden. <br />
      🐶 Each of the owners website, Ori and Dor has his own dog. Dor owns Pesto
      and Ori owns Rachel.<br />
      🐶 Pesto & Rachel is a brand that connecting people that loves dogs and
      food.<br />
      🐶 Enjoy Cooking 👨‍🍳<br />
    </p>

    <b-container fluid class="p-4 bg-dark">
      <b-row>
        <b-col>
          <img
            class="dogs-img"
            src="../assets/pesto.jpeg"
            alt="Image 1"
          /><img />
        </b-col>
        <b-col>
          <img
            class="dogs-img"
            src="../assets/rachel.jpeg"
            alt="Image 1"
          /><img />
        </b-col>
      </b-row>
    </b-container>
    <h1>Previous Assignments</h1>
    <h3>Assignment 3.1</h3>
    <a href="https://app.swaggerhub.com/apis-docs/dorlevi22/RecipeAPI/1.0.0"
      >🐶 Swagger Recipe API</a
    >
    <h3>Assignment 3.2</h3>
    <a
      href="https://github.com/Web-Development-Environments-2022/assignment3_2-316614726_316306323
"
    >
      🐶 Github Link
    </a>
  </div>
</template>

<script>
export default {
  name: "about",
  data() {
    return {};
  },
  validations: {},
  methods: {},
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 800px;
  margin-bottom: 50px;
}
.dogs-img {
  border: 5px solid white;
  width: 250px;
  height: 300px;
}
</style>
