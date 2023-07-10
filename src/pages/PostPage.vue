<template>
  <div>
    <h1>Post List Page</h1>
    <my-input
      v-model="searchQuery"
      placeholder="Search..."
    >
    </my-input>
    <div class="app__btns">
      <my-button
      @click="showDialog"
      >
      Create
      </my-button>
      <my-select 
        v-model="selectedSort"
        :options="sortOptions"
      >
      </my-select>
    </div>
    <!-- <my-button @click="fetchPosts">
      Fetch posts
    </my-button> -->
    <my-dialog v-model:show="dialogVisible">
      <post-form
        @create="createPost" 
      />
    </my-dialog>
    <post-list 
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      @addlike="addLike"
      @adddislike="addDisLike"
      v-if="!isPostLoading"
    />
    <div v-else>Loading</div>
    <div ref="observer" class="observer"></div>
    <!-- <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPages" 
        :key="page"
        class="page"
        :class="{
          'current-page': pageNumber === page
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
  </template>
  
  <script>
  import PostForm from "@/components/PostForm.vue"
  import PostList from "@/components/PostList.vue"
  import axios from "axios"
  
  export default {
    components: {
      PostForm, 
      PostList
    },
  
    data(){
      return {
        posts: [],
        dialogVisible: false,
        isPostLoading: false,
        selectedSort: '',
        searchQuery: '',
        page: 1,
        limit: 10,
        totalPages: 0,
        sortOptions: [
          {value: 'title', name: 'by title' },
          {value: 'body', name: 'by body'}
        ]
      }
    },
    
    methods: {
      createPost(post) {
        console.log(post)
        this.posts.push(post)
        this.dialogVisible = false;
      },
      removePost(post) {
        this.posts = this.posts.filter( p => p.id !== post.id)
      },
      addLike(post) {
        // console.log(post)
        this.posts.find(element => element.id === post.id).likes += 1;
      },
      addDisLike(post) {
        this.posts.find(element => element.id === post.id).disLikes += 1;
      },
      showDialog() {
        this.dialogVisible = true;
      },
      async fetchPosts() {
        try {
          this.isPostLoading = true;
          setTimeout( async() => {
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts?', {
              params: {
                _page: this.page,
                _limit: this.limit,
              }
            });
            this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = response.data;
            for (let i = 0; i < 10; i++) {
              Object.assign(this.posts[i], {'likes': 0})
              Object.assign(this.posts[i], {'disLikes': 0})
            }
            // console.log(response)
            this.isPostLoading = false
          }, 1000);
        } catch (e) {
          alert('Error')
        }
      },
      async loadMorePages() {
        try {
          this.page += 1;
          setTimeout( async() => {
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts?', {
              params: {
                _page: this.page,
                _limit: this.limit,
              }
            });
            this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = [...this.posts, ...response.data]
            for (let i = 0; i < 10; i++) {
              Object.assign(this.posts[i], {'likes': 0})
              Object.assign(this.posts[i], {'disLikes': 0})
            }
            // console.log(response)
            this.isPostLoading = false
          }, 1000);
        } catch (e) {
          alert('Error')
        } finally {
  
        }
      },
      // changePage(pageNumber) {
      //   this.page = pageNumber;
      // }
    },
  
    mounted() {
      this.fetchPosts();
      const options = {
        rootMargin: '0px',
        threshold: 1.0
      }
      const callback = (entries, observer) => {
        if (entries[0].isIntersecting && this.page < this.totalPages)
          // console.log('peresec div');
          this.loadMorePages()
      };
      const observer = new IntersectionObserver(callback, options);
      observer.observe(this.$refs.observer);
    },
    computed: {
      sortedPosts() {
        return [...this.posts].sort((post1, post2) => {
          return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
        })
      },
      sortedAndSearchedPosts() {
        return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
      }
    },
    // watch: {
    //   page() {
    //     this.fetchPosts();
    //   }
    // }
    // watch: {
    //   selectedSort(newValue) {
    //     this.posts.sort((post1, post2) => {
    //       return post1[newValue]?.localeCompare(post2[newValue])
    //     })
    //   }
    // }
  }
  </script>
  
  <style>
  .app__btns {
    display: flex;
    justify-content: space-between;
  }
  .page__wrapper {
    display: flex;
    margin-top: 15px;
    margin-left: auto;
    margin-right: auto;
    width: 6em
  }
  .page {
    border: 2px solid rebeccapurple;
    padding: 10px;
    margin-left: 2px;
    margin-right: 2px;
    border-radius: 6px;
  }
  .current-page {
    border: 3px solid rgb(77, 40, 116);
  }
  .observer {
    height: 30px;
    background-color: aquamarine;
  }
  </style>
  