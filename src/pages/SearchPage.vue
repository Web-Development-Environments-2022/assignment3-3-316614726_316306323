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
    <!-- <b-card class="mt-3 md-3" header="Form Data Result">
      <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
      <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
    </b-card> -->
  </div>
</template>

<script>
import { required, alpha } from "vuelidate/lib/validators";
export default {
  name: "search",
  data() {
    return {
      form: {
        query: "",
        cuisine: "",
        diet: "",
        intolerance: "",
        submitError: undefined,
      },
      errors: [],
      validated: false,
    };
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
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        console.log(this.$root.store.server_domain);
        console.log("search function entered!");
        // const response = await this.axios.post(
        //   // "https://test-for-3-2.herokuapp.com/user/Register",
        //   this.$root.store.server_domain + "/Register",

        //   {
        //     username: this.form.username,
        //     firstname: this.form.firstName,
        //     lastname: this.form.lastName,
        //     country: this.form.country,
        //     password: this.form.password,
        //     email: this.form.email,
        //   }
        // );
        // console.log(response);
        // this.$router.push("/login");
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
      this.Search();
    },
    onReset() {
      this.form = {
        query: "",
        cousine: "",
        diet: "",
        intolerance: "",
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
