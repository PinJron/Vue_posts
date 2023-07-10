<template>
  <div class="post">
    <div>
      <div><strong>Name</strong> {{  post.title }}</div>
      <div><strong>Description</strong> {{  post.body }}</div>
      <div class="marks">
        <div 
          style="margin-right: 10px;"
        >
          Likes {{ post.likes }}
        </div>
        <div
        >
          Dislikes {{ post.disLikes }}
        </div>
      </div>
    </div>
    <div class="post__btns">
      <my-button 
        @click="$emit('remove', post)"
      >
        Delete post
      </my-button>
      <my-button 
        @click="$emit('addlike', post)"
      >
        Like
      </my-button>
      <my-button
        class="mark"
        @click="$emit('adddislike', post)"
      >
        Dislike
      </my-button>
    </div>
  </div>
</template>
  
<script>
export default{
  props: {
    post: {
      type: Object,
      required: true
    }
  }
}
</script>
  
<style>
.post {
  padding: 15px;
  border: 2px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.post__btns {
  display: flex;
}
.marks {
  display: flex;
  margin-left: auto;
  margin-right: auto;
}
</style>