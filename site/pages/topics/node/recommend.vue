<template>
  <section class="main">
    <top-notice />
    <div class="container main-container is-white left-main">
      <div class="left-container">
        <topics-nav :nodes="nodes" :current-node-id="-1" />
        <topic-list :topics="topicsPage.results" :show-ad="true" />
        <pagination
          :page="topicsPage.page"
          url-prefix="/topics/node/recommend?p="
        />
      </div>
      <topic-side />
    </div>
  </section>
</template>

<script>
import TopicSide from '~/components/TopicSide'
import TopicsNav from '~/components/TopicsNav'
import TopicList from '~/components/TopicList'
import Pagination from '~/components/Pagination'
import TopNotice from '~/components/TopNotice'

export default {
  components: {
    TopicSide,
    TopicsNav,
    TopicList,
    Pagination,
    TopNotice
  },
  async asyncData({ $axios, query }) {
    try {
      const [user, nodes, topicsPage] = await Promise.all([
        $axios.get('/api/user/current'),
        $axios.get('/api/topic/nodes'),
        $axios.get('/api/topic/recommend/topics', {
          params: {
            page: query.p || 1
          }
        })
      ])
      return { user, nodes, topicsPage }
    } catch (e) {
      console.error(e)
    }
  },
  head() {
    return {
      title: this.$siteTitle('热门话题'),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$siteDescription()
        },
        { hid: 'keywords', name: 'keywords', content: this.$siteKeywords() }
      ]
    }
  }
}
</script>

<style lang="scss" scoped></style>
