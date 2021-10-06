<template>
  <div class="posts-index">
    <h1>Blog Posts</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <img :src="post.image" alt="" />
      <br />
      <br />
      <router-link :to="`/posts/${post.id}`">MORE DETAILS</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log(response.data);
      this.posts = response.data;
    });
  },
  methods: {},
};
</script>
