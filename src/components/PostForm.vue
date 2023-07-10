<template>
  <form @submit.prevent>
    <h4>Create Post</h4>
    <my-input
      v-model="post.title"
      type="text"
      placeholder="Name"
    ></my-input>
    <my-input
      v-model="post.body"
      type="text"
      placeholder="Descryption"
    ></my-input>

    <my-button
      style="align-self: flex-end; margin-top: 15px"
      @click="createPost"
    >
      Create
    </my-button>
  </form>
</template>

<script>
export default{
  data() {
    return {
      post: {
        id: 0,
        title: '',
        body: '',
        likes: 0,
        disLikes: 0
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.post.likes = 0;
      this.post.disLikes = 0;
      this.$emit('create', this.post);
      // this.post = {
      //   id: 0,
      //   title: '',
      //   body: '',
      //   likes: 0,
      //   disLikes: 0,
      // }
    },
  }
}

</script>

<style>
form {
  display: flex;
  flex-direction: column;
}
</style>
