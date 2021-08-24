<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <scroller class="content"
              ref="scroller"
              :probe-type="3"
              @scroll="getPosition"
              :pull-up-load="true" @pullingUp="loadMore">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control class="home-tab-control" :titles="titles" @tabClick="tabClick"></tab-control>
      <goods-list v-bind:goods="showGoods"></goods-list>
    </scroller>
    <backTop @click.native="backTopClick" v-show="isBackTOP"></backTop>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabcontrol/TabControl'
import GoodsList from "@/components/content/goods/GoodsList";
import HomeSwiper from 'views/home/childComps/HomeSwiper'
import RecommendView from 'views/home/childComps/RecommendView'
import  FeatureView from 'views/home/childComps/FeatureView'
import {getHomeMultidata,  getHomeGoods} from 'network/home'
import Scroller from "@/components/common/scroll/Scroller";
import backTop from "@/components/content/backTop/backTop";

  export default {
    name: "Home",
    components: {
      NavBar,
      Scroller,
      TabControl,
      HomeSwiper,
      RecommendView,
      FeatureView,
      GoodsList,
      backTop
    },
    data() {
      return {
        banners: [],
        recommends: [],
        titles: ['流行', '新款', '精选'],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType: 'pop',
        isBackTOP: false
      }
    },
    created() {
      // 首页创建完就可以请求数据
      this.getHomeMultidata();
      //  首页流行，新款,精选的数据
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell')
    },
    computed: {
      showGoods() {
        return  this.goods[this.currentType].list
      }
    },
    methods: {
      //事件监听
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },
      //返回顶部
      backTopClick() {
        //通过￥refs拿到组件中对象
        this.$refs.scroller.scrollTo(0, 0, 500)
      },
      //拿到y位置，显示backtop
      getPosition(position) {
        this.isBackTOP = -position.y > 1000
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
        //刷新上拉高度
        this.$refs.scroller.scroller.refresh()
      },
      //网络请求
      getHomeMultidata(){
        getHomeMultidata().then(res => {
          // console.log(res.data);
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        }).catch()
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1;
            getHomeGoods(type, page).then(res => {
              this.goods[type].list.push(...res.data.list);
              this.goods[type].page += 1;
              // this.goods = res.data;
              //完成上拉
              this.$refs.scroller.finishPullUp()

          })
        // console.log(this.goods['pop'].list)
        }
}

  }
</script>

<style scoped>
#home {
  padding-top: 44px;
  position: relative;
  height: 100vh;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 999;
  
}
.home-tab-control {
  position: sticky;
  top: 44px;
  z-index: 999;
}
.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  left: 0;
  right: 0;
  bottom: 49px;
}
</style>
