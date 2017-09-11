<template>
<Col :span="spanLeft" class="layout-menu-left">
          <Menu name="1" width="auto" class="dark">
            <div class="layout-logo-center">
              <a :href="baseUrl" :title="title">
                <img src="~/assets/logo/logo.png" :width="spanLeft < 5 ? '40px' : '90px'" :height="spanLeft < 5 ? '40px' : '90px'" :alt="title">
              </a>
            </div>
            <MenuItem :name="'1-' + index" v-for="(cat, index) in categories" :index="'1-' + index" :key="'1-' + index">
              <span class="layout-text">
                <a :href="baseUrl + keyword + '/' + cat.slug + '/'">
                  <Icon type="ios-keypad" :size="iconSize" :color="iconColor"></Icon>
                  {{ cat.title }} [{{ cat.post_count }}]
                </a>
              </span>
            </MenuItem>
            <MenuItem name="40" class="divided">
              <span class="layout-text" >
                <strong>
                  <a :href="baseUrl + catKey + '/1/'">
                    <Icon type="ios-keypad" :size="iconSize" :color="iconColor"></Icon>
                    All categories
                  </a>
                </strong>
              </span>
            </MenuItem>
            <MenuItem name="41" class="divided">
              <span class="layout-text">
                <a :href="baseUrl + searchKey + '/keyword/'">
                  <Icon type="ios-keypad" :size="iconSize" :color="iconColor"></Icon>
                  Keyword
                </a>
              </span>
            </MenuItem>
          </Menu>
        </Col>
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
      searchKey: process.env.SEARCH_KEYWORD,
      iconColor: '#EDE7F6'
    }
  },
  methods: {
    fetchData () {
      axios.get('/cats/0/').then(response => {
        this.categories = response.data
      }).catch(e => {
        console.log(e)
      })
    }
  },
  computed: {
    iconSize () {
      return this.spanLeft === 5 ? 14 : 24
    }
  },
  props: {
    spanLeft: Number
  },
  mounted () {
    this.fetchData()
  }
}
</script>

<style scoped>
.layout-logo-center {
  width: 90%;
  height: 5em;
  border-radius: 3px;
  display: block;
  margin: 25px auto;
  text-align: center;
  background: transparent;
}

.dark {
  background: #cb60b3;
  background: -moz-linear-gradient(-45deg, #cb60b3 0%, #ad1283 50%, #de47ac 100%);
  background: -webkit-linear-gradient(-45deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
  background: linear-gradient(135deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
}

.layout-menu-left {
  background: #cb60b3;
  background: -moz-linear-gradient(-45deg, #cb60b3 0%, #ad1283 50%, #de47ac 100%);
  background: -webkit-linear-gradient(-45deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
  background: linear-gradient(135deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
}

.layout-text {
  color: #EDE7F6;
}

a {
  color: #EDE7F6;
}

a:hover {
  color: #de47ac;
}

.divided {
  border-width: 1px;
  border-style: solid;
  border-color: #ff4cff #7986CB #7986CB #7986CB;
}
</style>