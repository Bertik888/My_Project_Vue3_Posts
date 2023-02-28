<template>
  <form @submit.prevent>
    <h4>Создание поста</h4>
    <my-input
      v-focus
      v-model="post.title"
      type="text" 
      placeholder="Название"
    />

    <my-input 
      v-model="post.body"
      type="text" 
      placeholder="Описание"
    />

      <!-- Потом удалить style="align-self: flex-end" и добавить class="btn" оличчие от общего btn, что тут надо добавить margin-top: 15px, поэтому возможно нужно будет добавить новый класс -->
    <my-button       
      style="align-self: flex-end; margin-top: 15px"  
      @click="createPost"
    >
      Создать
    </my-button>
  </form>
</template>

<script>
  export default {
    data() {
      return {
        post: {
          title: '',
          body: ''
        }
      }
    },

    methods: {
      createPost() {
        this.post.id = Date.now();
        this.$emit('create', this.post)
        this.post = {
          title: '',
          body: ''
        }
      }
    },    
  }
</script>

<style lang="scss" scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>

<!-- vbase - снипет для создания шаблона -->