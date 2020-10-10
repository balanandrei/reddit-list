<template>
  <div>
    <article class="PostItem" v-for="(post, i) in posts" :key="i">
      <div class="PostItem__Content">
        <figure>
          <img :src="post.data.thumbnail" />
        </figure>
        <p class="PostItem__Text">
          <a class="PostItem__Title" :href="url + post.data.permalink">
            <p>{{ post.data.title }}</p>
          </a>
          <a
            class="PostItem__Subreddit"
            :href="url + '/' + post.data.subreddit_name_prefixed"
            >{{ post.data.subreddit_name_prefixed }}</a
          >
        </p>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  name: "RedditListItem",
  props: ["posts", "url"],
};
</script>

<style scoped lang="scss">
.PostItem {
  display: flex;
  flex-direction: column;
  border: 1px solid #000;
  margin-bottom: 10px;

  &__Content {
    display: flex;
    flex-direction: row;
    align-items: center;
    background: rgb(148, 143, 232);
    background: linear-gradient(
      90deg,
      rgba(148, 143, 232, 1) 0%,
      rgba(173, 173, 204, 1) 35%,
      rgba(0, 212, 255, 1) 100%
    );
  }

  &__Text {
    display: flex;
    flex-direction: column;
  }

  &__Title,
  &__Subreddit {
    margin: 0 20px;
  }
}
</style>
