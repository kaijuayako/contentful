<template>
  <v-container fluid>
    <template v-if="posts.length">
      <ul v-for="(post, i) in posts" :key="i">
        <li>{{ post.fields.title }}</li>
        <li>
          <img
            :src="setEyeCatch(post).url"
            :alt="post.fields.image.fields.title"
            :aspect-ratio="16/9"
            width="400"
            height="225"
          >
        </li>
        <li>{{ post.fields.body }}</li>
        <li>{{ post.fields.publishDate }}</li>
        <span :is="draftChip(post)" />
        <li><nuxt-link :to="linkTo(post)">この記事を見る</nuxt-link></li>
      </ul>
    </template>
    <template v-else>
      投稿された記事はありません
    </template>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
import client from '~/plugins/contentful'
import draftChip from '~/components/posts/draftChip'
export default {
  components: {
    draftChip
  },
  computed: {
    ...mapGetters(['setEyeCatch', 'draftChip']),
    linkTo: () => (obj) => {
      return {
        name: 'posts-slug',
        params: {
          slug: obj.fields.slug
        }
      }
    }
  },
  async asyncData ({ env }) {
    let posts = []
    await client.getEntries({
      content_type: env.CTF_BLOG_POST_TYPE_ID,
      order: '-fields.publishDate'
    }).then(res => (posts = res.items)).catch(console.error)
    return { posts }
  }
}
</script>
