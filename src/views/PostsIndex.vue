<template>
  <div class="posts-index">
    <div class="container-fluid py-5">
      <h1 class="animate__animated animate__heartBeat animate__infinite animate__slower">Blog Posts</h1>
      <p class="col-md-8 fs-4">Read this stuff.</p>
      <button class="btn btn-primary btn-lg" type="button">Example button</button>
    </div>
    <div>
      Search:
      <input type="text" v-model="titleFilter" list="titles" />

      <datalist id="titles">
        <option v-for="post in posts" v-bind:key="post.title">{{ post.title }}</option>
      </datalist>
    </div>
    <br />
    <button v-on:click="sortAttribute = `title`">Sort by Title</button>
    &nbsp;
    <button v-on:click="sortAttribute = `created_at`">Sort by Date</button>
    &nbsp;
    <button v-on:click="sortAttribute = `user_id`">Sort by User</button>
    &nbsp;
    <button v-on:click="sortAttribute = `body`">Sort by Body Text</button>

    <div class="container">
      <div class="row row-cols-3">
        <div
          class="col"
          v-for="post in orderBy(filterBy(posts, titleFilter, 'title'), sortAttribute)"
          v-bind:key="post.id"
        >
          <div class="card" style="width: 18rem">
            <img :src="post.image" class="card-img-top" alt="..." />
            <div class="card-body">
              <h5 class="card-title">{{ post.title }}</h5>
              <p class="card-text">Created at: {{ relativeDate(post.created_at) }} Created by: {{ post.user_id }}</p>
              <router-link class="btn btn-primary" :to="`/posts/${post.id}`">More Details</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div
      is="transition-group"
      enter-active-class="animate__animated animate__rollIn"
      leave-active-class="animate__animated animate__rollOut"
    ></div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
import dayjs from "dayjs";
var relativeTime = require("dayjs/plugin/relativeTime");
dayjs.extend(relativeTime);

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      posts: [],
      titleFilter: "",
      sortAttribute: "title",
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log(response.data);
      this.posts = response.data;
    });
  },
  methods: {
    relativeDate: function (created_at) {
      return dayjs(created_at).fromNow();
    },
  },
};
</script>
