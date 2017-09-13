<template>
    <footer>
        <Row type="flex">
            <Col :span="3"></Col>
            <Col :span="6" class="align-center">
                <div>
                    <a :href="baseUrl"><img src="~/assets/logo/logo.png" width="50" height="50" :alt="siteName"></a>
                </div>
                <div>
                    <p><a :href="'https://twitter.com/' + twHandle"><i class="fa fa-twitter"></i></a>&nbsp;
                    <a :href="'https://www.facebook.com/' + fbHandle"><i class="fa fa-facebook"></i></a></p>
                    <address>
                        <p>&copy; 2017 <a :href="baseUrl">{{ siteName }}</a> | 
                        Developed by <a href="https://talaikis.com">Talaikis Inc.</a></p>
                    </address>					
                </div>
            </Col>
            <Col :span="3"></Col>
        </Row>
    </footer>
</template>

<script>
export default {
  name: 'footerComponent',
  data () {
    return {
      baseUrl: process.env.BASE_URL,
      twHandle: process.env.TWITTE_HANDLE,
      fbHandle: process.env.FACEBOOK_HANDLE,
      siteName: process.env.SITE_NAME
    }
  }
}
</script>

<style>
.align-center {
    text-align: center;
    font-size: 0.8em;
    padding: 5em 0 5em 0;
    color: #9ea7b4;
  }
</style>