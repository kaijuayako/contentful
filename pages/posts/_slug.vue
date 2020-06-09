<template>
    <div class="wrap">
        <v-breadcrumbs :items="breadcrumbs">
          <template #divider>
            <v-icon>mdi-chevron-right</v-icon>
          </template>
        </v-breadcrumbs>
        <template v-if="currentPost">
            {{ currentPost.fields.title }}
            <img
                :src="currentPost.fields.image.fields.file.url"
                :alt="currentPost.fields.image.fields.title"
                :aspect-ratio="16/9"
                width="700"
                height="400"
                class="mx-auto"
            />
            {{ currentPost.fields.publishDate }}<br>
            {{ currentPost.fields.body }}
        </template>

        <template v-else>
            お探しの記事は見つかりませんでした。
        </template>

        <div>
            <nuxt-link to="/"> ホームへ戻る </nuxt-link>
        </div>
    </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['setEyeCatch']),
    breadcrumbs () {
      const category = this.currentPost.fields.category
      return [
        { text: 'ホーム', to: '/' },
        { text: category.fields.name, to: '#' }
      ]
    }
  },
  async asyncData ({ payload, store, params, error }) {
    const currentPost = payload || await store.state.posts.find(post => post.fields.slug === params.slug)
    if (currentPost) {
      return { currentPost }
    } else {
      return error({ statusCode: 400 })
    }
  }
}
</script>
