<template>
<Row :span="20">
  <Col :span="3"></Col>
  <Col :span="14" class="align-center">
    <div class="pagination" v-if="paginatorType === 0">
      <a v-for="n in paginationRange" :class="activePage(n)" :href="baseUrl + 'page/' + n + '/'" @click="pageChanged(n)" class="page-link">{{ n }}</a>
    </div>
    <div class="pagination" v-if="paginatorType === 1">
      <a v-for="n in paginationRange" :class="activePage(n)" :href="baseUrl + keyword + '/' + value + '/page/' + n + '/'" @click="pageChanged(n)" class="page-link">{{ n }}</a>
    </div>
    <div class="pagination" v-if="paginatorType === 2">
      <a v-for="n in paginationRange" :class="activePage(n)" :href="baseUrl + catKey + '/' + n + '/'" @click="pageChanged(n)" class="page-link">{{ n }}</a>
    </div>
    <div class="pagination" v-if="paginatorType === 3">
      <a v-for="n in paginationRange" :class="activePage(n)" :href="baseUrl + searchKey + '/' + value + '/page/' + n + '/'" @click="pageChanged(n)" class="page-link">{{ n }}</a>
    </div>
  </Col>
  <Col :span="3"></Col>
</Row>
</template>

<script>
import Util from '../plugins/util.js'

export default {
  name: 'paginatorComponent',
  data () {
    return {
      baseUrl: process.env.BASE_URL,
      keyword: process.env.KEYWORD,
      catKey: process.env.CATEGORIES_KEY,
      searchKey: process.env.SEARCH_KEYWORD
    }
  },
  methods: {
    activePage (pageNum) {
      return this.currentPage === pageNum ? 'page-item active' : 'page-item'
    },
    pageChanged (pageNum) {
      this.$emit('page-changed', pageNum)
    }
  },
  computed: {
    lastPage () {
      if (this.totalPages) {
        return this.totalPages
      } else {
        return this.totalItems % this.itemsPerPage === 0
          ? this.totalItems / this.itemsPerPage
          : Math.floor(this.totalItems / this.itemsPerPage)
      }
    },
    paginationRange () {
      let start =
        this.currentPage - this.visiblePages / 2 <= 0
          ? 1 : this.currentPage + this.visiblePages / 2 > this.lastPage
            ? Util.lowerBound(this.lastPage - this.visiblePages + 1, 1)
            : Math.ceil(this.currentPage - this.visiblePages / 2)

      let range = []

      for (let i = 0; i < this.visiblePages && i < this.lastPage; i++) {
        range.push(start + i)
      }

      return range
    }
  },
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    value: {
      type: String,
      required: false
    },
    paginatorType: {
      type: Number,
      required: true
    },
    totalPages: Number,
    itemsPerPage: Number,
    totalItems: Number,
    visiblePages: {
      type: Number,
      default: 20,
      coerce: (val) => parseInt(val)
    }
  }
}
</script>

<style>
.pagination {
  display: block;
}

.pagination a {
  color: black;
  float: left;
  padding: 8px 16px;
  text-decoration: none;
}

.pagination a.active {
  background-color: #de47ac;
  color: #fff;
}

.align-center {
  text-align: center;
}
</style>