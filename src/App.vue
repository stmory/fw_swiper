<template>
  <div id="app">
    <fwSwiper>
      <div slot="content" style="background: red;" >
        11111
      </div>
      <div slot="content" style="background: blue;" >
        22222
      </div>
      <div slot="content" style="background: yellow;" >
        33333
      </div>
      <div slot="content" style="background: pink;" >
        44444
      </div>
      <div slot="content" style="background: red;" >
        11111
      </div>
      <div slot="content" style="background: blue;" >
        22222
      </div>
      <div slot="content" style="background: yellow;" >
        33333
      </div>
      <div slot="content" style="background: pink;" >
        44444
      </div>
    </fwSwiper>
  </div>
</template>

<script>
import fwSwiper from './components/fw_swiper.vue'

export default {
  name: 'App',
  components: {
    fwSwiper
  }
}
</script>

