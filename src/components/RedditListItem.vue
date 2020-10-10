<template>
  <div>
    <article
      class="PostItem"
      v-for="(post, i) in posts"
      :key="`${i}-${post.data.id}`"
    >
      <a :href="url + post.data.permalink">
        <h3>{{ post.data.title }}</h3>
        <img :src="post.data.thumbnail" />
      </a>
      <a :href="url + '/' + post.data.subreddit_name_prefixed">{{
        post.data.subreddit_name_prefixed
      }}</a>
    </article>
  </div>
</template>

<script>
export default {
  name: "RedditListItem",
  props: ['posts'],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
