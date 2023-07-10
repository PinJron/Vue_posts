<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>
  
<script>

export default {
  name: 'my-button'
}
</script>

<style scoped>
.btn {
  margin-top: 10px;
  padding: 10px 15px;
  margin-right: 5px;
  background-color: rgb(238, 245, 245);
  color: rebeccapurple;
  border: 2px solid rebeccapurple;
  border-radius: 6px;
}
</style>