<template>
  <div class="posts-show">
    <h2>{{ post.title }}</h2>
    <img :src="post.image" alt="" />
    <h3>{{ post.body }}</h3>
    <p>Created by: {{ post.user_id }}</p>
    <p>Created: {{ relativeDate(post.created_at) }}</p>
    <p></p>
    <div v-if="$parent.getUserId() == post.user_id">
      <router-link :to="`/posts/${post.id}/edit`">Edit Post</router-link>
      <br />
      <br />
      <button v-on:click="destroyPost()">Delete Post</button>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
var relativeTime = require("dayjs/plugin/relativeTime");
dayjs.extend(relativeTime);

import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
  methods: {
    destroyPost: function () {
      if (confirm("Are you sure about that, friend?")) {
        axios.delete(`/posts/${this.post.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
          window.alert("Post successfully DESTROYED!!!");
        });
      }
    },
    relativeDate: function (created_at) {
      return dayjs(created_at).fromNow();
    },
  },
};
</script>
