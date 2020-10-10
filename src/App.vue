<template>
  <div id="app">
    <h2>Reddit feed</h2>
    <section class="Posts">
      <RedditListItem :posts="posts" :url="url"/>
    </section>
    <div ref="observeScroll"></div>
    <button @click="loadMore">Load more</button>
  </div>
</template>

<script>
import RedditListItem from './components/RedditListItem.vue'

export default {
  name: "App",
  components: { RedditListContainer ,RedditListItem },
  data() {
    return {
      posts: [],
      limitNumber: 25,
      url: "https:///www.reddit.com",
      subreddit: "aww",
      afterId: null,
      test: null,
    };
  },
  methods: {
    getData() {
      let apiUrl = `https://api.reddit.com/r/${this.subreddit}/top.json?limit=${
        this.limitNumber
      }${this.afterId ? "&after=" + this.afterId : ""}`;

      fetch(apiUrl)
        .then((response) => response.json())
        .then((response) => {
          this.afterId = response.data.after;
          this.posts = [...this.posts, ...response.data.children];
        })
        .then(() => this.observeScroll());
        
    },

    observeScroll() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio === 1) {
            observer.unobserve(entry.target);
            console.log("intersectionRatio", entry.intersectionRatio);
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
</style>
