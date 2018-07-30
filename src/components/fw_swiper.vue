<template>
  <div class="fw-swiper-main">
    <div id="fw-swiper-main-slide" ref="slide">
      <div class="fw-swiper-main-slide-content" ref="slide-content">
        <slot name="content" class="fw-swiper-main-slide-page">
        </slot>
      </div>
    </div>
    <div class="fw-swiper-main-btn-left" type="button" @click="last">&lt;</div>
    <div class="fw-swiper-main-btn-right" type="button" @click="next">&gt;</div>
  </div>
</template>

<script>
export default{
  data() {
    return {
      // 当前页
      nowslide: {
        nowPage: 0,
        nowPX: 0
      },
      finish: true,
      thisItemWidth: this.itemWidth,
      thisItemHeight: this.itemHeight,
      contentLeft: 0, // content 移动后的left
      restWidth: 0, // content 右边剩余的width
    }
  },
  props: {
    itemWidth: {
      type: [Number, String],
      default: 200
    },
    itemHeight: {
      type: [Number, String],
      default: 100
    },
  },
  computed: {
    main: function() {
      // 轮播主体
      return this.$refs.slide
    },
    width: function() {
      // 获取宽度
      return this.main.offsetWidth
    },
    count: function() {
      // 获取轮播页数量
      return this.$slots.content.length
    }
  },
  mounted() {
    // 初始化轮播组件
    this.initSlide()
  },
  methods: {
    handleClick(selectedIndex) {
      this.selectedIndex = selectedIndex
      this.$emit('onSelect', selectedIndex)
    },
    stringToFloat(str) {
      let num = parseFloat(str.split('%').join(''))/100
      return num
    },
    initSlide() {
      let slides = this.$slots.content
      if (typeof(this.itemWidth) === 'string') {
        this.thisItemWidth = this.width * this.stringToFloat(this.itemWidth)
      }
      for (let i = 0; i < slides.length; i++) {
        slides[i].elm.style.width = this.thisItemWidth + 'px'
        slides[i].elm.style.float = 'left'
        slides[i].elm.style.height = '100%'
      }
      let inner = this.main.innerHTML
      this.main.firstElementChild.style.width = `${this.count * this.thisItemWidth}px`
      this.main.firstElementChild.style.left = `0`
      this.main.firstElementChild.style.position = `relative`
      this.main.firstElementChild.style.height = typeof(this.thisItemHeight) === 'string' ? this.thisItemHeight : `${this.thisItemHeight}px`
      let mWidth = parseInt(this.$refs['slide-content'].style.width.split('px').join(''))
      this.contentLeft = parseInt(this.$refs['slide-content'].style.left.split('px').join(''))
      this.restWidth = mWidth - this.width + this.contentLeft > 0 ? mWidth - this.width + this.contentLeft : 0
    },
    last() {
      if (!this.finish) {
        return
      }
      this.finish = false
      let page = this.main.firstElementChild
      if (this.contentLeft === 0) {
        this.finish = true
        return
      }else if(- this.contentLeft < this.thisItemWidth) {
        page.style.transition = 'all .3s'
        page.style.left = '0px'
        page.addEventListener('transitionend', () => {
          page.style.transition = 'none'
          this.setRestWidth()
        }, false)
      }else {
        page.style.transition = 'all .3s'
        page.style.left = this.contentLeft + this.thisItemWidth + 'px'
        page.addEventListener('transitionend', () => {
          page.style.transition = 'none'
          this.setRestWidth()
        }, false)
      }
    },
    next() {
      if (!this.finish) {
        return
      }
      this.finish = false
      let page = this.main.firstElementChild
      if (this.restWidth === 0) {
        this.finish = true
        return
      }else if(this.restWidth < this.thisItemWidth) {
        page.style.transition = 'all .3s'
        page.style.left = this.contentLeft - this.restWidth + 'px'
        page.addEventListener('transitionend', () => {
          page.style.transition = 'none'
          this.setRestWidth()
        }, false)
      }else {
        page.style.transition = 'all .3s'
        let pageLeft = parseInt(this.$refs['slide-content'].style.left.split('px').join(''))
        page.style.left = pageLeft - this.thisItemWidth + 'px'
        page.addEventListener('transitionend', () => {
          page.style.transition = 'none'
          this.setRestWidth()
        }, false)
      }
    },
    setRestWidth() {
      let mWidth = parseInt(this.$refs['slide-content'].style.width.split('px').join(''))
      this.contentLeft = parseInt(this.$refs['slide-content'].style.left.split('px').join(''))
      this.restWidth = mWidth - this.width + this.contentLeft > 0 ? mWidth - this.width + this.contentLeft : 0
      this.finish = true
    }
  }
}
</script>
<style scoped>
.fw-swiper-main{
  position: relative;
  padding: 10px 30px;
}
#fw-swiper-main-slide{
  position: relative;
  left: 0;
  right: 0;
  height: 100%;
  overflow: hidden;
}
.fw-swiper-main-btn-left, .fw-swiper-main-btn-right{
  font-size: 26px;
  top: 50%;
  margin-top: -26px;
  height: 52px;
  line-height: 52px;
  cursor: pointer;
  position: absolute;
  user-select:none;
}
.fw-swiper-main-btn-left{
  left: 10px;
}
.fw-swiper-main-btn-right{
  right: 10px;
}
</style>