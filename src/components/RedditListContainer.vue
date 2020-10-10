<template>
  <div>
    <section class="Posts">
      <RedditListItem :posts="posts" :url="url" />
    </section>
  </div>
</template>

<script>
import RedditListItem from './RedditListItem.vue'

export default {
  name: "RedditListContainer",
  components: { RedditListItem },
  props: ["posts", "url"],
};
</script>
