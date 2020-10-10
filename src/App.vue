<template>
  <div id="app">
    <div>Reddit</div>
    <ol>
      <li v-for="(post, i) in posts" :key="`${i}-${post.data.id}`">
        <a :href="url + post.data.permalink">
          <h3>{{ post.data.title }}</h3>
          <img :src="post.data.thumbnail" />
          <div>{{ post.data.subreddit_name_prefixed }}</div>
        </a>
      </li>
    </ol>
    <div ref="observeScroll"></div>
    <button @click="loadMore">Load more</button>
  </div>
</template>

<script>

export default {
  name: "App",
  data() {
    return {
      posts: [],
      limitNumber: 25,
      url: "https:///www.reddit.com",
      subreddit: "aww",
      afterId: null,
    };
  },
  methods: {
    getData() {
      let apiUrl = `https://api.reddit.com/r/${this.subreddit}/top.json?limit=${this.limitNumber}&after=${this.afterId}`;

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
            console.log('intersectionRatio', entry.intersectionRatio);
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

<style lang="scss"></style>
