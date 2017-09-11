<template>
  <section>
    <div class="layout" :class="spanLeft < 5 ? 'layout-hide-text' : ''">
      <Row type="flex">
        <header-component :spanLeft="spanLeft"></header-component>
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
import Header from '../components/Header.vue'

export default {
  name: 'mainLayout',
  data () {
    return {
      spanLeft: 5,
      spanRight: 19
    }
  },
  methods: {
    toggleClick () {
      if (this.spanLeft === 5) {
        this.spanLeft = 2
        this.spanRight = 22
      } else {
        this.spanLeft = 5
        this.spanRight = 19
      }
    }
  },
  components: {
    'footer-component': Footer,
    'header-component': Header
  }
}
</script>

<style>
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

.layout-hide-text .layout-text {
  display: none;
}

.layout-header {
  height: 60px;
  background: #fff;
  box-shadow: 0 1px 1px rgba(0,0,0,.1);
}

a {
  color: #de47ac;
}

a:hover {
  color: #cb60b3;
}
</style>