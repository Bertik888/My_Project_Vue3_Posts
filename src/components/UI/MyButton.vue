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

<style lang="scss" scoped>
.btn {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;

  // align-self: flex-end; // Потом добавить сюда из PostForm my-button
}

</style>