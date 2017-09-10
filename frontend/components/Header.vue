<template>
  <header>
    <el-row>
      <el-col :span="24">
        <el-menu theme="dark" class="el-menu-demo" mode="horizontal" @select="handleSelect">
          <el-menu-item index="1">
            <a class="navbar-brand" :href="baseUrl" :title="title">
              <img src="~/assets/logo/logo.png" width="30" height="30" alt="Logo">
              {{ title }}
            </a>
          </el-menu-item>
          <el-submenu index="2">
            <template slot="title">Categories</template>
            <el-menu-item v-for="(cat, index) in categories" :index="'2-' + index" key="cats">
              <a class="dropdown-item" :href="baseUrl + keyword + '/' + cat.slug + '/'">
                {{ cat.title }} [{{ cat.post_count }}]
              </a>
            </el-menu-item>
            <hr />
            <el-menu-item index="2-41">
              <strong><a :href="baseUrl + catKey + '/1/'" class="dropdown-item">All categories</a></strong>
            </el-menu-item>
          </el-submenu>
          </el-submenu index="3">
            <template slot="title">Trending</template>
            <el-menu-item index="3-1">
              <a class="dropdown-item" :href="baseUrl + searchKey + '/keyword/'">Keyword</a>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-col>
    </el-row>
  </header>
</template>

<script>
import axios from 'axios'

export default {
  name: 'headerComponent',
  data () {
    return {
      categories: this.categories,
      baseUrl: process.env.BASE_URL,
      logoAlt: process.env.SITE_NAME,
      title: process.env.SITE_NAME,
      keyword: process.env.KEYWORD,
      catKey: process.env.CATEGORIES_KEY,
      searchKey: process.env.SEARCH_KEYWORD
    }
  },
  methods: {
    fetchData () {
      axios.get('/cats/0/').then(response => {
        this.categories = response.data
      }).catch(e => {
        console.log(e)
      })
    },
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    }
  },
  mounted () {
    this.fetchData()
  }
}
</script>
