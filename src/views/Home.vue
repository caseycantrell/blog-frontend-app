<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2 v-if="!$parent.isLoggedIn()">Please login to continue.</h2>
  </div>
</template>

<style scoped>
h2 {
  color: red;
}
</style>

<script>
export default {
  data: function () {
    return {
      message: "Welcome to this dope af blog page.",
    };
  },
  created: function () {},
  methods: {},
};
</script>
