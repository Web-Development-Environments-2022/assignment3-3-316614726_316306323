<template>
  <div class="container">
    <h1 class="title">Search Recipe!</h1>
    <p id="lastSearchP" v-if="lastSearch">
      🔎 Your Last Search Was:&nbsp;&nbsp;
      <b
        ><i>{{ lastSearch }}</i></b
      >&nbsp;&nbsp;🔎
    </p>
    <b-form @submit.prevent="onSearch" @reset.prevent="onReset">
      <b-form-group
        id="input-group-query"
        label-cols-sm="3"
        label="What do you want to cook?:"
        label-for="query"
      >
        <b-form-input
          id="query"
          v-model="$v.form.query.$model"
          type="text"
          :state="validateState('query')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.query.length">
          Query must be non-empty!
        </b-form-invalid-feedback>
      </b-form-group>

      <div>
        <b-button v-b-toggle.collapse-1 variant="primary">Filter</b-button>
        <b-collapse id="collapse-1" class="mt-2">
          <b-card>
            <b-form-group
              id="input-group-CuisineGroup"
              label-cols-sm="3"
              label="Cuisine:"
              label-for="cuisine"
            >
              <b-form-select
                id="cuisine"
                v-model="$v.form.cuisine.$model"
                :options="cuisines"
              ></b-form-select>
            </b-form-group>
            <b-form-group
              id="input-group-DietGroup"
              label-cols-sm="3"
              label="Diet:"
              label-for="diet"
            >
              <b-form-select
                id="diet"
                v-model="$v.form.diet.$model"
                :options="diets"
              ></b-form-select>
            </b-form-group>
            <b-form-group
              id="input-group-IntoleranceGroup"
              label-cols-sm="3"
              label="Intolerance:"
              label-for="intolerance"
            >
              <b-form-select
                id="intolerance"
                v-model="$v.form.intolerance.$model"
                :options="intolerances"
              ></b-form-select>
            </b-form-group>
          </b-card>
          <b-card>
            <b-form-select
              v-model="form.sort"
              :options="sortOptions"
              value="form.sort"
              size="sm"
              class="mt-3"
            ></b-form-select>
          </b-card>
        </b-collapse>
      </div>

      <b-form-group
        label="Please choose number of returned results:"
        v-slot="{ ariaDescribedby }"
      >
        <b-form-radio-group
          v-model="form.number"
          :options="amountOptions"
          :aria-describedby="ariaDescribedby"
          name="plain-inline"
          plain
        ></b-form-radio-group>
      </b-form-group>

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Search</b-button
      >
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Search failed: {{ form.submitError }}
    </b-alert>

    <RecipePreviewList
      v-if="searchClicked && recipes.length > 0"
      title="Search Results"
      state="search"
      :recipesData="recipes"
      :key="searchClicked"
    />
    <b-modal id="noResultsModal" title="Not Found Any Recipe!"
      >There is no recipe that meets the search requirements. Please search
      again more accurately!
    </b-modal>
  </div>
</template>

<script>
import { required, alpha } from "vuelidate/lib/validators";
import RecipePreviewList from "../components/RecipePreviewList";
import cuisines from "../assets/cuisines";
import diets from "../assets/diets";
import intolerances from "../assets/intolerances";

export default {
  name: "search",
  components: { RecipePreviewList },
  data() {
    return {
      form: {
        query: "",
        cuisine: "",
        diet: "",
        intolerance: "",
        number: 5,
        sort: "",
        submitError: undefined,
      },
      cuisines: [{ value: null, text: "", disabled: true }],
      diets: [{ value: null, text: "", disabled: true }],
      intolerances: [{ value: null, text: "", disabled: true }],
      amountOptions: [
        // options
        { text: "5", value: 5 },
        { text: "10", value: 10 },
        { text: "15", value: 15 },
      ],
      sortOptions: [
        { value: "", text: "Sort By", disabled: true },
        { value: "time", text: "Time" },
        { value: "popularity", text: "Popularity" },
      ],
      lastSearch: "",
      errors: [],
      validated: false,
      searchClicked: 0,
      recipes: [],
    };
  },
  mounted() {
    this.cuisines.push(...cuisines);
    this.diets.push(...diets);
    this.intolerances.push(...intolerances);

    this.lastSearch = localStorage.getItem("lastSearch");
  },
  validations: {
    form: {
      query: {
        required,
      },
      cuisine: {
        alpha,
      },
      diet: {
        alpha,
      },
      intolerance: {
        alpha,
      },
    },
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        const paramsList = {};
        const allParams = [
          "query",
          "cuisine",
          "diet",
          "intolerance",
          "number",
          "sort",
        ];
        allParams.forEach((param) => {
          if (this.form[param]) {
            paramsList[param] = this.form[param];
          }
        });
        let response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/recipes/SearchRecipes",
          {
            params: paramsList,
          }
        );
        this.recipes.push(...response.data);
        this.searchClicked += 1;
        if (response.data.length == 0) {
          this.$bvModal.show("noResultsModal");
        }
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSearch() {
      // console.log("register method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }

      this.recipes = [];
      localStorage.setItem("lastSearch", this.form["query"]);
      this.lastSearch = this.form["query"];
      this.Search();
    },
    onReset() {
      this.form = {
        query: "",
        cuisine: "",
        diet: "",
        intolerance: "",
        number: 5,
        sort: "",
        submitError: undefined,
      };
      this.errors = [];
      this.validated = false;
      this.searchClicked = 0;
      this.recipes = [];

      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 70%; // 650px;
}

#lastSearchP {
  border-style: solid;
  border-width: medium;
  border-color: black;
  background-color: #ff7f50;
  max-width: 500px;
}
</style>
