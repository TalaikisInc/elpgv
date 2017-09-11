<template>
  <section>
    <div class="layout" :class="{'layout-hide-text' : spanLeft < 5}">
      <Row type="flex">
        <Col :span="spanLeft" class="layout-menu-left">
          <Menu name="1" width="auto" class="dark">
            <div class="layout-logo-center">
              <a :href="baseUrl" :title="title">
                <img src="~/assets/logo/logo.png" width="60" height="60" :alt="title">
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
        <Col :span="spanRight">
          <div class="layout-header">
            <Button type="text" @click="toggleClick">
              <Icon type="navicon" size="32"></Icon>
            </Button>
          </div>
          <div class="layput-content">
            <div class="layout-content-main">
              <nuxt/>
            </div>
            <footer-component></footer-component>
          </div>
        </Col>
      </Row>
    </div>
  </section>
</template>

<script>
import Footer from '../components/Footer.vue'
import axios from 'axios'

export default {
  name: 'mainLayout',
  data () {
    return {
      categories: this.categories,
      baseUrl: process.env.BASE_URL,
      logoAlt: process.env.SITE_NAME,
      title: process.env.SITE_NAME,
      keyword: process.env.KEYWORD,
      catKey: process.env.CATEGORIES_KEY,
      searchKey: process.env.SEARCH_KEYWORD,
      spanLeft: 4,
      spanRight: 20,
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
    },
    toggleClick () {
      if (this.spanLeft <= 5) {
        this.spanLeft = 2
        this.spanRight = 22
      } else {
        this.spanLeft = 4
        this.spanRight = 20
      }
    }
  },
  computed: {
    iconSize () {
      return this.spanLeft === 5 ? 14 : 24
    }
  },
  mounted () {
    this.fetchData()
  },
  components: {
    'footer-component': Footer
  }
}
</script>

<style scoped>
  .layout {
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
    border-radius: 4px;
    overflow: hidden;
  }

  .layout-content {
    min-height: 200px;
    margin: 15px;
    overflow: hidden;
    background: #fff;
    border-radius: 4px;
  }

  .layout-content-main {
    padding: 40px;
  }

  .layout-menu-left {
    background: #cb60b3;
    background: -moz-linear-gradient(-45deg, #cb60b3 0%, #ad1283 50%, #de47ac 100%);
    background: -webkit-linear-gradient(-45deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
    background: linear-gradient(135deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#cb60b3', endColorstr='#de47ac',GradientType=1 );
  }

  .layout-header {
    height: 60px;
    background: #fff;
    box-shadow: 0 1px 1px rgba(0,0,0,.1);
  }

  .layout-logo-center {
    width: 90%;
    height: 60px;
    border-radius: 3px;
    display: block;
    margin: 25px auto;
    text-align: center;
    background: transparent;
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

  .ivu-col {
    transition: width .2s ease-in-out;
  }

  .dark {
    background: #cb60b3;
    background: -moz-linear-gradient(-45deg, #cb60b3 0%, #ad1283 50%, #de47ac 100%);
    background: -webkit-linear-gradient(-45deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
    background: linear-gradient(135deg, #cb60b3 0%,#ad1283 50%,#de47ac 100%);
  }
</style>