---
title: Vue轮播图插件vue-awesome-swiper的使用
date: 2017-07-20 11:37:20
tags:
---
![logo](vue-awesome-swiper.png)
<strong>Vue轮播图插件vue-awesome-swiper的引入和简单使用</strong>
[https://github.com/sansansine/vogue.git](https://github.com/sansansine/vogue.git)
<!--more-->
## 1、Use Setup
> npm install vue-awesome-swiper --save

## 2、main.js-import
>import VueAwesomeSwiper from 'vue-awesome-swiper'
>Vue.use(VueAwesomeSwiper)

## 3、.vue页面中声明参数

```ruby
import { swiper, swiperSlide } from 'vue-awesome-swiper'
export default{
  name: 'one',
  components: {
    swiper,
    swiperSlide
  },
  data () {
    return {
      // currentDate: new Date(),
      items: ['../static/banner1.jpg', '../static/banner2.jpg', '../static/banner3.jpg'],
      swiperOption: {
        autoplay: 3000,
        pagination: '.swiper-pagination',
        slidesPerView: 'auto',
        centeredSlides: true,
        paginationClickable: true,
        onSlideChangeEnd: swiper => {
          this.page = swiper.realIndex + 1
          this.index = swiper.realIndex
        }
      },
      swiperSlides: [1, 2, 3, 4, 5]
    }
  },
  computed: {
    swiper () {
      return this.$refs.mySwiper.swiper
    }
  },
  mounted () {
  }
}```
