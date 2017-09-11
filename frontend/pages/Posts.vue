<template>
  <div>
    <ad-component></ad-component>
    <Row v-for="(chunk, index) in chunkPosts" :key="'p-' + index" class="posts-row">
      <Col :span="2"></Col>
      <Col :span="8" v-for="(post, i) in chunk" :key="index + i" class="posts-col">
        <Card :bordered="false">
          <div v-if="post.image">
            <a :href="baseUrl + post.slug + '/'"><img :src="imgBaseUrl + post.image" :alt="post.title" class="img-fluid"></a>
          </div>
          <div>
            <p><small>
            <a :href="baseUrl + keyword + '/' + post.category_id.Slug + '/'">{{ post.category_id.Title }}</a>
              | {{ post.date | formatDate }}
            </small></p>
          </div>
          <div>
            <h2><a :href="baseUrl + post.slug + '/'">{{ post.title }}</a></h2>
            <p v-if="post.content">{{ post.content | truncate }}</p>
          </div>
        </Card>
      </Col>
      <Col :span="2"></Col>
      <Col :span="20" v-if="index === (3 || 7)">
        <ad-component></ad-component>
      </Col>
    </Row>
    <paginator-component v-once :totalPages="calcPages" :paginatorType="paginatorType" value="" :currentPage="page" :itemsPerPage="itemsPerPage" :totalItems="posts[0].total_posts">
    </paginator-component>
  </div>
</template>

<script>
import chunk from '../plugins/chunk'
import Paginator from '../components/Paginator.vue'
import Ads from '../components/Ads.vue'
import axios from 'axios'

export default {
  data () {
    return {
      posts: [],
      baseUrl: process.env.BASE_URL,
      imgBaseUrl: process.env.IMG_URL,
      title: process.env.SITE_NAME,
      keyword: process.env.KEYWORD,
      page: null,
      paginatorType: 0,
      itemsPerPage: 20
    }
  },
  asyncData ({ req, params, error }) {
    return axios.get('/posts/' + (Number(params.page) || '0') + '/')
      .then((response) => {
        return { posts: response.data, page: Number(params.page) || 0 }
      })
      .catch((e) => {
        error({ statusCode: 500, message: e })
      })
  },
  components: {
    'paginator-component': Paginator,
    'ad-component': Ads
  },
  computed: {
    chunkPosts () {
      return chunk(this.posts, 2)
    },
    calcPages () {
      const pages = Math.floor(this.posts[0].total_posts / 20)
      return pages <= 250 ? pages : 250
    }
  },
  head () {
    return {
      title: Number(this.$route.params.page) ? 'Page ' + Number(this.$route.params.page) + ' | ' + this.title : this.title
    }
  }
}
</script>

<style>
.img-fluid {
  width: 100%;
}

.posts-row {
  background: #eee;
  padding: 2em;
}

.posts-col {
  padding: 1em;
}

p {
    font-size: 1.5em;
    font-face: Roboto, Arial;
}

h2 {
  font-size: 2.5em;
  font-face: Roboto, Arial;
}
</style>
