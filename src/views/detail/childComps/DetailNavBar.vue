<template>
    <div>
        <nav-bar class="top"> 
            <div slot="left" class="back-icon" @click="backClick">
                <img src="@/assets/img/detail/back.svg" alt="">
            </div>
            <div slot="center" class="title">
                <div class="title-item"
                 v-for="(item,index) in titles"
                 :class="{active: index==currentIndex}"
                 @click="titleClick(index)">{{item}}</div>
            </div>
        </nav-bar>
    </div>
</template>
<script>
import NavBar from '@/components/commom/navbar/NavBar'
export default {
    name: 'DetailNavBar',
    data() {
        return {
          titles:['商品','参数','评论','推荐'],
          currentIndex: 0
        }
    },
    methods: {
        titleClick(index) {
            this.currentIndex = index
        },
        backClick() {
            this.$router.back()
        }
    },
    components: {
        NavBar
    }
}
</script>
<style scoped>
    .title {
        display: flex;
        font-size: 20px;
    }
    .title-item {
        flex: 1;
    }
    .active {
        color: red;
    }
    .back-icon img{
        margin-top: 12px;
        height: 20px;
        
    }
</style>