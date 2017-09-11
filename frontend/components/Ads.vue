<template>
  <Row :span="20">
    <Col :span="20">
      <adsense
        :ad-client="adClient"
        :ad-slot="adSlot"
        ad-style="display: block"
        ad-format="auto">
      </adsense>
    </Col>
  </Row>
</template>

<script>
import VueAdsense from './VueAdsense.vue'

export default {
  name: 'adsComponent',
  data () {
    return {
      adClient: process.env.ADSENSE_AD_CLIENT,
      adSlot: process.env.ADSENSE_AD_SLOT
    }
  },
  components: {
    'adsense': VueAdsense
  }
}
</script>
