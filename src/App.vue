<template>
  <div id="app">
    <div id="nav">
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand
          >🍴 Pesto & Rachel - It's All About The Taste 🍴</b-navbar-brand
        >

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item :to="{ name: 'main' }">Main</b-nav-item>
            <b-nav-item :to="{ name: 'search' }">Search 🔎</b-nav-item>
            <b-nav-item-dropdown
              :disabled="!$root.store.username ? true : false"
              text="Personal"
            >
              <b-dropdown-item :to="{ name: 'favorite' }"
                >Favorites ❤</b-dropdown-item
              >
              <b-dropdown-item :to="{ name: 'private' }"
                >Private 🔒</b-dropdown-item
              >
              <b-dropdown-item :to="{ name: 'family' }"
                >Family Recipes 👪</b-dropdown-item
              >
              <b-dropdown-item @click="updateModal" v-b-modal.modal-lg
                >Create New Recipe 📝</b-dropdown-item
              >
            </b-nav-item-dropdown>
            <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-item-dropdown right>
              <!-- Using 'button-content' slot -->
              <template #button-content>
                <em>User 👤</em>
              </template>

              <b-dropdown-item
                v-if="!$root.store.username"
                :to="{ name: 'register' }"
                >Register</b-dropdown-item
              >
              <b-dropdown-item
                v-if="!$root.store.username"
                :to="{ name: 'login' }"
                >Login</b-dropdown-item
              >
              <b-dropdown-item v-if="$root.store.username" @click="Logout"
                >Logout</b-dropdown-item
              >
            </b-nav-item-dropdown>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <CreateRecipeModal v-if="showModal" />
    <div id="main">
      <img id="header" src="./assets/app-header.jpg" />
      <router-view />
    </div>
  </div>
</template>

<script>
import CreateRecipeModal from "./components/CreateRecipeModal";
export default {
  name: "App",
  components: {
    CreateRecipeModal,
  },
  data() {
    return {
      showModal: false,
    };
  },
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
    updateModal(e) {
      e.preventDefault();
      this.showModal = true;
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  min-height: 100vh;
}

#header {
  width: 100%;
  height: 600px;
  z-index: -10;
}
/*
#nav {
  overflow: hidden;
  position: fixed; 
  top: 0;
  width: 100%;
}

#main {
  margin-top: 30px;
}
*/
</style>
