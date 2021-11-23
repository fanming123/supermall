<template>
    <div id="detail">
        <detail-nav-bar></detail-nav-bar>
        <detail-swiper :topImages="topImages"></detail-swiper> 
    </div>
</template>
<script>
import DetailNavBar from '@/views/detail/childComps/DetailNavBar'
import {getDetail ,Shop} from '../../network/detail'
import DetailSwiper from '@/views/detail/childComps/DetailSwiper'
import DetailShopInfo from '@/views/detail/childComps/DetailShopInfo'

export default {
    name: "Detail",
    data() {
        return {
            iid: null,
            topImages: []
        }
    },
    components: {
        DetailNavBar,
        DetailSwiper,
        DetailShopInfo
      
    },
    created() {
        this.iid = this.$route.params.iid,
        getDetail(this.iid).then( res => {
            console.log(res)
            this.topImages = res.data.result.itemInfo.topImages
            console.log(this.topImages)
        })
    }
}
</script>
<style scoped>

</style>
    
