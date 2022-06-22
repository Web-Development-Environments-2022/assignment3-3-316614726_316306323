<template>
  <div class="container">
    <h1 class="title">Search Recipe!</h1>
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
              <b-form-input
                type="text"
                id="cuisine"
                v-model="$v.form.cuisine.$model"
              >
              </b-form-input>
            </b-form-group>
            <b-form-group
              id="input-group-DietGroup"
              label-cols-sm="3"
              label="Diet:"
              label-for="diet"
            >
              <b-form-input type="text" id="diet" v-model="$v.form.diet.$model">
              </b-form-input>
            </b-form-group>
            <b-form-group
              id="input-group-IntoleranceGroup"
              label-cols-sm="3"
              label="Intolerance:"
              label-for="intolerance"
            >
              <b-form-input
                type="text"
                id="intolerance"
                v-model="$v.form.intolerance.$model"
              >
              </b-form-input>
            </b-form-group>
          </b-card>
        </b-collapse>
      </div>

      <b-form-group
        label="Please choose number of returned results:"
        v-slot="{ ariaDescribedby }"
      >
        <b-form-radio-group
          v-model="form.number"
          :options="form.options"
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
      v-if="searchClicked"
      title="Search Results:"
      state="search"
      :searchParams="params"
    />
  </div>
</template>

<script>
import { required, alpha } from "vuelidate/lib/validators";
import RecipePreviewList from "../components/RecipePreviewList";
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
        options: [
          { text: "5", value: 5 },
          { text: "10", value: 10 },
          { text: "15", value: 15 },
        ],
        submitError: undefined,
      },
      errors: [],
      validated: false,
      searchClicked: false,
    };
  },
  computed: {
    params: function() {
      return {
        query: this.form.query,
        cuisine: this.form.cuisine,
        diet: this.form.diet,
        intolerance: this.form.intolerance,
        number: this.form.number,
      };
    },
  },
  validations: {
    form: {
      query: {
        required,
        alpha,
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
  mounted() {
    // console.log("mounted");
    // console.log($v);
    this.searchClicked = false;
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search(formFields) {
      try {
        this.searchClicked = true;
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
      // console.log("register method go");
      // let formFields = this.form;
      // this.onReset();
      // this.form = formFields;
      // this.$forceUpdate();
      this.Search();
    },
    onReset() {
      this.searchClicked = false;
      this.form = {
        query: "",
        cousine: "",
        diet: "",
        intolerance: "",
        number: 5,
        options: [
          { text: "5", value: 5 },
          { text: "10", value: 10 },
          { text: "15", value: 15 },
        ],
        submitError: undefined,
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>
