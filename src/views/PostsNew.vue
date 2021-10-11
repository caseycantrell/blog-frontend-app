<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>Create Post</h1>
      <img v-if="status" :src="`https://http.cat/${status}`" alt="" />
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="newPostParams.title" />
      </div>
      <div>
        <label>Image:</label>
        <input type="text" v-model="newPostParams.image" />
      </div>
      <div>
        <label>Body:</label>
        <input type="text" v-model="newPostParams.body" />
        <br />
        <small>{{ 500 - newPostParams.body.length }} characters remaining.</small>
      </div>
      <br />
      <input type="submit" value="Create" />
      &nbsp;
      <input type="reset" value="Reset" />
    </form>
    <br />
    newPostParams: {{ newPostParams }}
  </div>
</template>
<style scoped>
img {
  width: 500px;
}
</style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: {
        body: "",
      },
      errors: [],
      posts: [],
      status: null,
    };
  },
  methods: {
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "Post created successfully!";
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.status = error.response.status;
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
