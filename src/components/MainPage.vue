<template>
  <div class="container header">
    <div class="row header__row">
      <div class="col-12 col-lg-4 ">
        <div class="input-group">
          <div class="input-group-prepend">
            <span class="input-group-text search">
              <img src="../assets/search.svg" alt />
            </span>
          </div>
          <input
            type="text"
            class="form-control"
            placeholder="Filter by author..."
            v-model.trim="searchText"
            @input="searchPost()"
          />
          <div class="invalid-tooltip">
            Please choose a unique and valid username.
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div
        v-for="item of posts"
        :key="item.id"
        class="col-12 col-sm-6 col-lg-4 col-xl-4 cards"
      >
        <Card :post="item"></Card>
      </div>
    </div>
  </div>
</template>

<script>
import Card from "../components/Card.vue";
import axios from "axios";
export default {
  name: "MainPage",
  components: {
    Card
  },
  data() {
    return {
      posts: null,
      users: null,
      postsFull: null,
      searchText: null
    };
  },
  mounted() {
    this.buildFinalPosts();
  },
  methods: {
    getPosts() {
      return axios.get("https://jsonplaceholder.typicode.com/posts");
    },
    getUsers() {
      return axios.get("https://jsonplaceholder.typicode.com/users");
    },
    buildFinalPosts() {
      Promise.all([this.getPosts(), this.getUsers()]).then(result => {
        this.posts = result[0].data;
        this.users = result[1].data;

        this.posts.forEach(post => {
          this.users.forEach(user => {
            if (post.userId == user.id) {
              post.author = user.name;
            }
          });
        });
        this.postsFull = this.posts;
      });
    },
    searchPost() {
      if (this.searchText == null) {
        this.posts = this.postsFull;
        return;
      }
      this.posts = this.postsFull.filter(x =>
        x.author.toLowerCase().includes(this.searchText.toLowerCase())
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";
@import "node_modules/bootstrap-vue/src/index.scss";

.header {
  &__row {
    display: flex;
    justify-content: center;
  }

  .search {
    img {
      height: 24px;
    }
  }
}

.cards {
  margin-top: 20px;
}
</style>
