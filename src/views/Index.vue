<template>
  <div>
    <div class="top">
      <router-link to="/city" tag="li" class="city ">
        <i class="iconfont icon-compass"></i>
        <span>{{ name }}</span>
      </router-link>
      <input type="text" placeholder="搜索" class="search" @focus="search"><button class="iconfont icon-search" @click="search"></button>

      <router-link to="/my" tag="li" class="city ">
        <i class="iconfont icon-comments"></i>
        <span>消息</span>
      </router-link>
    </div>
    <div v-if="looplist.length">

      <swiper
        :options="options"
        v-if="looplist.length"
        style="margin-top: .5rem;
    width: 100%;"
    class="loopswiper"
    classname="loopswiper"
      >
        <div class="swiper-slide" v-for="data in looplist" :key="data._id">
          <img :src="data.pic" style="width:100%"/>
          <!-- {{ data }} -->

        </div>
      </swiper>
    </div>

    <asidebar></asidebar>

    <h3><span>今日推荐</span></h3>
    <swiper :options="navoptions" v-if="swiperlist.length" class="listswiper" classname="listswiper" >
      <div
        class="swiper-slide nav"
        v-for="data in swiperlist"
        :key="data._id"
        @click="detailClick(data.id)"
      >
        <img :src="data.icon" />
        <span>{{ data.name }}</span>
      </div>
    </swiper>

    <mainbar></mainbar>
  </div>
</template>
<script>
import swiper from "@/components/swiper";
import asidebar from "@/views/Index/asidebar";
import mainbar from "@/views/Index/mainbar";
import Axios from "axios";
import { Indicator, Swipe, SwipeItem } from "mint-ui";

import Vue from "vue";
Vue.component(Swipe.name, Swipe);
Vue.component(SwipeItem.name, SwipeItem);
export default {
  data() {
    return {
      looplist: [],
      swiperlist: [],
      name: "",
      navoptions: {
        slidesPerView: 3,
        spaceBetween: 3,
        freeMode: true
      },
      options: {
        // direction: 'vertical',
        loop: true,
        autoplay: {
          delay: 2500,
          disableOnInteraction: false
        },
        pagination: {
          el: ".swiper-pagination",
          clickable: true,
        }
      }
    };
  },
  components: {
    swiper,
    asidebar,
    mainbar
  },
    methods:{
      detailClick(id){
          this.$router.push(`/detail/${id}`)
      },
      search(){
        this.$router.push('/search')
      }
    },
  mounted() {
    Indicator.open({
      text: "加载中...",
      spinnerType: "fading-circle"
    });
    Axios.get('/api/slider').then(res => {
      this.looplist = res.data.data;
      Indicator.close();
    });

    if(!localStorage.getItem("name")){
      this.name = "定位中.."
    }else{
       var a = localStorage.getItem("name");
      console.log(a);
      this.name = a;
    }

    Axios.get('/api/swiper').then(res => {
      this.swiperlist = res.data.data;
    });
  }
};
</script>
<style lang="scss" scoped>
.top {
  position: fixed;
  left: 0;
  top: 0;
  background: white;
  width: 100%;
  height: .5rem;
  line-height: .5rem;
  z-index: 999;
  text-align: center;
}
.mint-searchbar {
  height: .5rem;
}
.mint-search {
  height: 100%;
  overflow: hidden;
}
   .swiper-wrapper{
      margin-top: 0;
    }
    .nav{
        width: 1rem;
        height: 1rem;
      img{
        width: 1rem;
        height: 1rem;
        margin: 0 auto;
        text-align: center;

      }
      span{
        width: 1rem;
        height: .25rem;
        background: rgba($color: #000000, $alpha: .5);
        position: absolute;
        bottom: 0;
        left: .1rem;
        text-align: center;
        color: white;
      }
    }
    .mint-swipe{
      height: 2rem;
      margin-top: .5rem;
    }
    .mint-swipe-indicator.is-active{
      background: skyblue;
    }
    .city{
      width: .3rem;
      height: 100%;
      flex: 10%;
      color: white;
      font-size: .12rem;
      display: flex;
      flex-direction: column;
      i{
        display: block;
        width: .3rem;
        height: .2rem;
        line-height: .2rem;
        margin: 0 auto;
        margin-top: .1rem;
      }
      span{
        display: block;
        height: .2rem;
        line-height: .2rem;
        text-align: center;
      }
    }
    .top{
       background: rgb(192, 163, 34);
       display:flex;
    }
h3 {
  height: 0.4rem;
  line-height: 0.4rem;
  span {
    border-left: 0.03rem solid rgb(192, 163, 34);
    margin-left: 0.15rem;
    box-sizing: border-box;
    width: 50%;
    padding-left: 0.06rem;
  }
}
/deep/.swiper-pagination{
  text-align: center;
}
.search{
  border-radius: .1rem;
  border: 0;
  padding: 0;
  height: .3rem;
  width: 2.3rem;
  margin: 0 auto;
  margin-top: .1rem;
  padding: .1rem;
  font-size: .12rem;
  border:1px solid #ccc;
  color: #999;
}
.icon-search{
  position: fixed;
  right: .9rem;
  top: .15rem;
  border: 0;
  background: none;
  padding: 0;
}
</style>
