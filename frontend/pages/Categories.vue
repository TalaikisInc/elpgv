<template>
  <div>
    <ad-component></ad-component>
    <Row>
      <Col :span="20">
        <h1>Categories<span v-if="page > 0">, page {{ page }}</span></h1>
      </Col>
    </Row>
    <Row v-for="(chunk, index) in chunkCats" :key="index">
      <Col :span="5" v-for="(cat, i) in chunk" :key="i">
        <h3><a :href="baseUrl + keyword + '/' + cat.slug + '/'">{{ cat.title }}</a> [{{ cat.post_count }}]</h3>
      </Col>
      <Col :span="20" v-if="index === (3 || 7)">
        <ad-component></ad-component>
      </Col>
    </Row>
    <paginator-component v-once :totalPages="calcPages" :paginatorType="paginatorType" value="" :currentPage="page" :itemsPerPage="itemsPerPage" :totalItems="categories[0].total_cats">
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
      categories: [],
      baseUrl: process.env.BASE_URL,
      title: process.env.SITE_NAME,
      keyword: process.env.KEYWORD,
      page: null,
      paginatorType: 2,
      itemsPerPage: 40
    }
  },
  asyncData ({ req, params, error }) {
    return axios.get('/cats/' + (Number(params.page) || '0') + '/')
      .then((response) => {
        return { categories: response.data, page: Number(params.page) || 0 }
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
    chunkCats () {
      return chunk(this.categories, 4)
    },
    calcPages () {
      const pages = Math.floor(this.categories[0].total_cats / 40)
      return pages <= 250 ? pages : 250
    }
  },
  head () {
    return {
      title: Number(this.$route.params.page) ? 'Page ' + Number(this.$route.params.page) + ' Categories | ' + this.title : this.title
    }
  }
}
</script>

<style>
</style>
