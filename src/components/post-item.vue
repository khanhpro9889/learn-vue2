<template>
  <div class="post-item">
    <div class="flex justify-between">
      <h3>{{ item.title }}</h3>
      <div>
        <button type="button" v-on:click="handleDeletePost(item.id)">
          Delete
        </button>
        <button type="button" v-on:click="handleEditPost(item)">Edit</button>
      </div>
    </div>
    <p>{{ item.body }}</p>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  methods: {
    handleDeletePost(postId) {
      this.$emit("deletePost", postId);
    },
    handleEditPost(postId) {
      this.$emit("editPost", postId);
    },
    handleCancelEdit() {
      this.$emit("handleCancelEdit");
    },
  },
};
</script>

<style scoped>
.post-item {
  padding: 16px;
  background-color: #eee;
  margin-block: 16px;
}
.flex {
  display: flex;
}
.justify-between {
  justify-content: space-between;
}
</style>
