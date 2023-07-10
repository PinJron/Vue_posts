<template>
  <div v-if="posts.length > 0">
    <transition-group name="postList">
      <postItem
        v-for="post in posts" 
        :post="post"
        :key = "post.id"
        @remove ="$emit('remove', post)"
        @addlike ="$emit('addlike', post)"
        @adddislike ="$emit('adddislike', post)"
      />
    </transition-group>
  </div>
  <div v-else="posts.length === 0" style="margin-top: 10px;">
    <strong style="color: rgb(196, 91, 26);">Posts list is undefined</strong>
  </div>
  <!-- <my-button @click="printbla(posts)">
    DEBUG =(
  </my-button> -->
</template>

<script>
import PostItem from "@/components/PostItem.vue"
export default {
  components: {PostItem},
  methods: {
    // printbla(posts) {
    //   console.log(posts.length);
    //   // print(posts.lenght)
    // }
  },
  props: {
    posts: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>
.post {
  padding: 15px;
  border: 2px solid rgb(90, 13, 183);
  margin-top: 15px;
  border-radius: 6px;
}
.postList-item {
  display: inline-block;
  margin-right: 10px;
}
.postList-enter-active,
.postList-leave-active {
  transition: all 0.5s ease;
}
.postList-enter-from,
.postList-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.postList-move {
  transition: transform 0.8s ease;
}
</style>
