<template>
  <div>
    <!-- Авторизация -->
    <!-- <h1>{{ $store.state.likes }}</h1> -->
    <!-- <h1>{{ $store.state.isAuth ? 'Пользователь авторизован' : 'Авторизуйтесь, чтобы использовать сервис' }}</h1>
    <h1>{{ $store.getters.doubleLikes }}</h1>
    <div>
      <my-button @click="$store.commit('incrementLikes')">Лайк</my-button>
      <my-button @click="$store.commit('decrementLikes')">Дизлайк</my-button>
    </div> -->

    <h1>Страница с постами</h1>

    <my-input 
      :model-value="searchQuery"
      @update:model-value="setSearchQuery"
      placeholder="Поиск..."
      v-focus
    />

    <div class="app__btns">
      <my-button
        @click="showDialog"
      >
        Создать пост
      </my-button>
      <my-select
        :model-value="selectedSort"
        @update:model-value="setSelectedSort"
        :options="sortOptions"      
      />      
    </div>   
    
    <my-dialog v-model:show="dialogVisible">
      <post-form 
        @create="createPost"
      />
    </my-dialog>

    <post-list 
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"         
    />

    <div v-else>Идет загрузка...</div>

    <div 
      v-intersection="loadMorePosts" 
      class="observer"
    >

    </div>

    <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>

  </div> 
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import MyButton from '@/components/UI/MyButton.vue';
import axios from 'axios';
import MySelect from '@/components/UI/MySelect.vue';
import MyInput from '@/components/UI/MyInput.vue';
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex';


export default {
  components: {
    PostList,
    PostForm,
    MyButton,
    MySelect,
    MyInput
  },

  data() {
    return {
      dialogVisible: false,       
    }
  },

  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),

    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),

    createPost(post) {      
      this.posts.push(post);
      this.dialogVisible = false;      
    },

    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },

    showDialog() {
      this.dialogVisible = true;
    }   
  },

  mounted() {
    this.fetchPosts();
  },

  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPostsLoading: state => state.post.isPostsLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions
    }),

    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
    })
  },

  watch: {
  },

  
  
}
</script>

<style>

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}
</style>

<!-- // Single file components -->