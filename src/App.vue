<template>
  <div id="app" class="wrapper">
    <post-form
      v-on:handleSubmit="handleSubmit"
      :post="selectedPost"
      v-on:handleCancelEdit="selectedPost = undefined"
    ></post-form>
    <template v-for="item in postsList">
      <post-item
        :item="item"
        :key="item.id"
        v-on:deletePost="deletePost"
        v-on:editPost="startEditPost"
      ></post-item>
    </template>
  </div>
</template>

<script>
import PostItem from "./components/post-item.vue";
import PostForm from "./components/post-form.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    "post-item": PostItem,
    "post-form": PostForm,
  },
  data: () => {
    return {
      postsList: [],
      selectedPost: undefined,
    };
  },
  methods: {
    async fetchPostsList() {
      const res = await axios.get("http://localhost:3000/posts");
      this.postsList = res.data.slice(0, 10);
    },
    async deletePost(postId) {
      await axios.delete(`http://localhost:3000/posts/${postId}`);
      this.fetchPostsList();
    },
    async handleSubmit(body) {
      if (this.selectedPost) {
        await axios.patch(
          `http://localhost:3000/posts/${this.selectedPost.id}`,
          {
            ...body,
            author: "typicode",
          }
        );
      } else {
        await axios.post("http://localhost:3000/posts", {
          ...body,
          author: "typicode",
        });
      }
      this.fetchPostsList();
    },
    startEditPost(post) {
      this.selectedPost = post;
    },
  },
  created() {
    this.fetchPostsList();
  },
};
</script>

<style>
* {
  padding: 0px;
  margin: 0px;
  box-sizing: border-box;
}
.wrapper {
  padding-inline: 50px;
}
</style>
