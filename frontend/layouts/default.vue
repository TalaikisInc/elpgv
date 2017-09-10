<template>
  <div>
    <header-component></header-component>
    <nuxt/>
    <footer-component></footer-component>
  </div>
</template>

<script>
import Header from '../components/Header.vue'
import Footer from '../components/Footer.vue'

export default {
  data () {
    return { }
  },
  components: {
    'header-component': Header,
    'footer-component': Footer
  }
}
</script>

<style src="~/node_modules/element-ui/lib/theme-default/index.css"></style>
