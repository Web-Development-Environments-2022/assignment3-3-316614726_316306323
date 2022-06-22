<template>
  <div class="container">
    <Login />
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
import Login from "../components/Login";
export default {
  name: "LoginPage",
  components: { Login },
  data() {
    return {};
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
