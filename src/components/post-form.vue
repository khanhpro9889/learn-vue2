<template>
  <form v-on:submit.prevent="handleSubmit">
    <div>
      <label for="title">Title</label>
      <input name="title" v-model="title" />
    </div>
    <div>
      <label for="body">Body</label>
      <textarea name="body" v-model="body"></textarea>
    </div>
    <input type="submit" :value="post ? 'Edit post' : 'Add new post'" />
    <button v-show="post ? true : false" v-on:click="handleCancelEdit">
      Cancel
    </button>
  </form>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: false,
    },
  },
  data() {
    return {
      title: "",
      body: "",
    };
  },
  methods: {
    handleSubmit() {
      this.$emit("handleSubmit", { title: this.title, body: this.body });
      this.title = "";
      this.body = "";
      if (this.post) {
        this.$emit("handleCancelEdit");
      }
    },
    handleCancelEdit() {
      this.$emit("handleCancelEdit");
    },
  },
  watch: {
    post: function (newVal) {
      // watch it
      if (newVal) {
        this.title = newVal.title;
        this.body = newVal.body;
      }
    },
  },
};
</script>

<style></style>
