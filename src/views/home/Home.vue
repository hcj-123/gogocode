<template>
  <div id="home" >
<!--    这里主要写逻辑-->
    <nav-bar class="nav-bar"><div slot="center">购物街</div></nav-bar>

    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="scrollContent"
            :pull-up-load="true"
            @pullingUp="loadMore">
        <div>
            <home-swiper :banners="banners"/>
        </div>

      <home-remmend :recomment="recommends"></home-remmend>
      <home-feature />
      <tab-control class="tab-control"
                   :titles="['流行', '新款', '精选']"
                   @tabClick="tabClick"/>
      <goods-list :goods="goods[currentType].list"/>
    </scroll>
<!--    组件中要想实现点击事件，必须要加.native-->
    <back-top @click.native="backClick" v-show="isShowBackTop"/>

  </div>
</template>

<script>


  import HomeSwiper from './childComps/HomeSwiper'
  import HomeRemmend from './childComps/HomeRecommend'
  import HomeFeature from './childComps/HomeFeature'


  import NavBar from '../../components/common/navbar/NavBar'
  import TabControl from '../../components/content/tabControl/TabControl'
  import GoodsList from '../../components/content/goods/GoodsList'
  import Scroll from '../../components/common/scroll/Scroll'
  import BackTop from '../../components/content/backTop/BackTop'

  import {getHomeMultidata,getHomeGoods} from '../../network/home'




  export default {
    name: "Home",
    data(){
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]}
        },
        currentType:'pop',
        isShowBackTop: false,
        isTabFixed: false
      }
    },
    components: {
      HomeSwiper,
      HomeRemmend,
      HomeFeature,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    created() {
      // 1、请求多个数据，在banner中使用过
      this.getHomeMultidata();

      // 2、请求商品数据
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell')
    },

    methods: {
      tabClick(index){
        switch (index) {
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell';
            break;

        }
      },
      getHomeMultidata(){
        getHomeMultidata().then(res => {
          console.log(res);
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type){
        const page = this.goods[type].page+1;
        getHomeGoods(type,page).then(res => {
          console.log(res);
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1;

            // 完成上拉加载更多
            this.$refs.scroll.scroll.finishPullUp();
            this.$refs.scroll.scroll.refresh();
        })
      },
      backClick(){
        this.$refs.scroll.scroll.scrollTo(0, 0 ,500)
      },
      scrollContent(position){
            // console.log(position);
            this.isShowBackTop = (-position.y) > 100;
      },
       loadMore(){
           console.log('上拉加载更多');
           this.getHomeGoods(this.currentType);
           this.$refs.scroll.scroll.refresh();
        }
    },
  }
</script>

<style scoped>
    #home{
      /*padding-top: 44px;*/
      /*  height: 100vh;*/
      position: relative;

    }
    .nav-bar{
      background-color: var(--color-tint);
      color: #f6f6f6;

      position: fixed;
      left: 0;
      right: 0;
      top: 0;
      z-index: 9;
    }
    .content {

        /*overflow: hidden;*/
        position: absolute;
        top: 44px;
        bottom: 50px;
        left: 0;
        right: 0;

    }

  .tab-control{
    position: sticky;
    top:44px;
    /*  position: relative;*/
    /*  z-index: 9;*/
  }

</style>
