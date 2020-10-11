<template>
  <div id="app">
    <h2>Reddit feed</h2>
    <RedditListContainer :posts="posts" :url="url" />
    <div ref="observeScroll"></div>
    <footer class="Footer">
      <div class="Loader" v-if="isLoading"></div>
    </footer>
  </div>
</template>

<script>
import RedditListContainer from "./components/RedditListContainer.vue";

export default {
  name: "App",
  components: { RedditListContainer },
  data() {
    return {
      posts: [],
      limitNumber: 25,
      url: "https:///www.reddit.com",
      subreddit: "aww",
      afterId: null,
      test: null,
      isLoading: false,
    };
  },
  methods: {
    getData() {
      this.isLoading = true;
      let apiUrl = `https://api.reddit.com/r/${this.subreddit}/top.json?limit=${
        this.limitNumber
      }${this.afterId ? "&after=" + this.afterId : ""}`;

      fetch(apiUrl)
        .then((response) => response.json())
        .then((response) => {
          this.afterId = response.data.after;
          this.posts = [...this.posts, ...response.data.children];
          this.isLoading = false;
        })
        .then(() => this.observeScroll());
    },

    observeScroll() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio === 1) {
            observer.unobserve(entry.target);
            this.getData();
          }
        });
      });
      observer.observe(this.$refs.observeScroll);
    },

    loadMore() {
      this.getData();
    },
  },
  created() {
    this.getData();
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Roboto+Condensed");

html,
body {
  font-family: "Roboto", sans-serif;
}

#app {
  font-family: "Roboto", sans-serif;
}

a {
  color: #000;
  text-decoration: none;
}

figure {
  margin: 0;
}

img {
  display: block;
}

.Footer {
  display: flex;
  margin: 20px 0;
}

.Loader {
  border: 6px solid #f3f3f3;
  border-top: 6px solid #a09fd9;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 2s linear infinite;
  margin-left: 20px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
