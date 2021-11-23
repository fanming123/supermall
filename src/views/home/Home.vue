<template>
    
    <div id="home">
        <nav-bar class="home-nav-bar"><div slot="center">购物街</div></nav-bar>
        <tab-control ref = "tabcontrol1" 
            :titles = "['流行','新款','精选']" 
            class="tab-control" 
            v-show="isShow"
            @tabClick="tabClick"></tab-control>
        <better-scroll class="content" 
                        ref="scroll" 
                        :probeType="3"
                        @scroll="contentScroll" 
                        @pullingUp="loadMore"
                      > 
            <home-swiper :banners = "banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
            <home-recommend :recommends = "recommends"></home-recommend>
            <tab-control ref = "tabcontrol2" 
            :titles = "['流行','新款','精选']" 
            class="tab-control"
            @tabClick="tabClick"
           ></tab-control>
            <goods-list :goods = "showGoods" ></goods-list>
        </better-scroll>
        <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
            
    </div>
   
</template>
<script>
import NavBar from '@/components/commom/navbar/NavBar'

import { getHomeMultidata ,
         getHomeGoods
 } from '@/network/home'

import HomeSwiper from '@/views/home/childComps/HomeSwiper'
import HomeRecommend from '@/views/home/childComps/HomeRecommend'
import TabControl from '@/components/commom/tabcontrol/TabControl'
import GoodsList from '@/components/content/goods/GoodsList'
import BetterScroll from '@/components/commom/betterscroll/BetterScroll'
import BackTop from '@/components/content/backtop/BackTop'

export default {
    name: "Home",
    components: {
        NavBar,
        HomeSwiper,
        HomeRecommend,
        TabControl,
        GoodsList,
        BetterScroll,
        BackTop
    },
    data() {
        return {
            banners: [],
            recommends: [],
            goods: {
                'pop' : {page: 0, list: []},
                'new' : {page: 0, list: []},
                'sell' : {page: 0, list: []},
            },
            currentType: 'pop',
            bscroll: null,
            isShowBackTop: true,
            positionY: {
                type: Number,
                default: 0
            },
            tabOffSetTop: {
                type: Number,
                default: 0
            },
            isShow: false,
            saveY: 0 
        }
    },
    created() {
        //1.请求多个数据
        this.getHomeMultidata(),
        //2.请求商品数据
        this.getHomeGoods('pop'),
        this.getHomeGoods('new'),
        this.getHomeGoods('sell')
        //监听图片加载完成
    },
    mounted() {
        this.$bus.$on("itemImgLoad", ()=>{
            this.$refs.scroll.bscroll.refresh()
        })
    },
    methods: {
        //网络请求相关方法
         getHomeMultidata() {
            getHomeMultidata().then(result => {
            this.banners = result.data.data.banner.list;
            this.recommends = result.data.data.recommend.list;
            })
        },
         getHomeGoods(type) {
            const page = this.goods[type].page + 1;
            getHomeGoods(type,page).then(res => {
            this.goods[type].list.push(...res.data.data.list);
            this.goods[type].page += 1;
            this.$refs.scroll.bscroll.finishPullUp()
            })
        },
        //事件监听相关方法
        tabClick(index){
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
            this.$refs.tabcontrol1.currentIndex = index;
            this.$refs.tabcontrol2.currentIndex = index;
        },
        backClick(){
            this.$refs.scroll.bscroll.scrollTo(0,this.positionY,200);
        },
        contentScroll(position) {
            this.isShowBackTop = -position.y > 1000;
            this.positionY = -position.y;
            this.isShow = (-position.y) > this.tabOffSetTop
        },
        loadMore() {
            this.getHomeGoods(this.currentType)
        },
        swiperImageLoad() {
            this.tabOffSetTop = this.$refs.tabcontrol2.$el.offsetTop;
        }
    },
    computed: {
        showGoods() {
            return this.goods[this.currentType].list; 
        }
    },
    activated() {
        this.$refs.scroll.bscroll.scrollTo(0,this.saveY,0);
        this.$refs.scroll.bscroll.refresh();
    },
    deactivated() {
        this.saveY = this.$refs.scroll.bscroll.y
    }
}
</script>
<style scoped>
    #home {
        height: 100vh;
    }
    .home-nav-bar {
        background-color: blueviolet;
        color: #fff;
       position: fixed;
        left: 0;
        right: 0;
        top: 0;
        z-index: 9; 
    }
    .tab-control {
        background-color: white;
        position: sticky;
        top: 44px;
        z-index: 9;
    }
    .content {
        position: relative;
        top: 44px;
        bottom: 49px;
        left: 0;
        right: 0;
        height: 600px;
    }
   
</style>
