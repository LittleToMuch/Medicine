<template>
  <div>
    <mt-header title="热销药材">
      <router-link to="/" slot="left">
        <mt-button icon="back"></mt-button>
      </router-link>
    </mt-header>
    <ul>
      <li @click="handleClick('黄芪')">
        <img src="http://news.image.yaocaimaimai.com/FnUJqa-OXXFhswgbXLjAfSR-wadG" alt="">
        <h4>黄芪</h4>
        <span>Astragalus root</span>
      </li>
      <li @click="handleClick('白及')">
        <img src="http://news.image.yaocaimaimai.com/FvjBk49GoVTfcMKuVtYOArBexjsh" alt="">
        <h4>白及</h4>
        <span>Common Bletilla Tuber</span>
      </li>
      <li @click="handleClick('金银花')">
        <img src="http://news.image.yaocaimaimai.com/Fv2_KmzpVQ84clXCS9zFWTcgWNED" alt="">
        <h4>金银花</h4>
        <span>Dried Honeysuckle Flower</span>
      </li>
      <li @click="handleClick('党参')">
        <img src="http://news.image.yaocaimaimai.com/FsR8lq08ChS0j1OEilLpI2AgE30g" alt="">
        <h4>党参</h4>
        <span>Codonopsis</span>
      </li>
      <li @click="handleClick('百合')">
        <img src="http://news.image.yaocaimaimai.com/Fi8hz4Hc4fs4hI-RKC8l3Jw_xrMF" alt="">
        <h4>百合</h4>
        <span>Liy Bulb</span>
      </li>
      <li class="last" @click="handleClick('白鲜皮')">
        <img src="http://news.image.yaocaimaimai.com/FqWFBMFHyxSzpygMLlzo9xjFtwUn" alt="">
        <h4>白鲜皮</h4>
        <span>Denesefruit Pittany Root</span>
      </li>
    </ul>
  </div>
</template>
<script>
  import Axios from 'axios'
    export default {
        methods: {
            handleClick(name) {
                Axios.get('/api/shop/shopDetails',{params:{name:name}}).then(res => {
                    console.log(res.data[0].id)
                    this.$router.push(`/detail/${res.data[0].id}`)
                    // return Axios.get('/api/shop/shopDetail',{params:{id:res.data[0].id}})
                })
            }
        },
        beforeMount() {
            this.$store.commit('hideTabbar')
        },
        beforeDestroy() {
            this.$store.commit('showTabbar')
        },
    }
</script>
<style lang="scss" scoped>
  body {
    .mint-header {
      background: white;
      color: black;
      border-bottom: 1px solid #ccc;
      height: .5rem;
    }

    ul {
      background: white;
      width: 87%;
      margin: 0 auto;
      margin-top: .1rem;

      li {
        height: 125px;
        position: relative;
        margin-top: .1rem;

        h4 {
          position: absolute;
          top: .5rem;
          left: .3rem;
          font-size: .16rem;

        }

        span {
          position: absolute;
          top: .8rem;
          left: .3rem;
          font-size: .16rem;
          font-style: normal;
        }
      }
    }

    .last {
      margin-bottom: .1rem;
    }
  }
</style>
