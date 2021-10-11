<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newUserParams.name" />
        <br />
        <small>{{ 50 - newUserParams.name.length }} characters remaining</small>
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
        <br />
        <small v-if="newUserParams.email.length == 0">You must provide an email</small>
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
        <br />
        <small v-if="newUserParams.password.length < 2">Password must be at least 2 characters</small>
        <small v-if="newUserParams.password.length > 20">Password cannot be more than 20 characters</small>
      </div>
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
        <br />
        <small v-if="newUserParams.password !== newUserParams.password_confirmation">Passwords must match!</small>
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
<style scoped>
small {
  color: red;
}
</style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: {
        name: "",
        email: "",
        password: "",
        password_confirmation: "",
      },
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
