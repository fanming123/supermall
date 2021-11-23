<template>
    <div class="wrapper" ref="wrapper">
        <div class="content">
            <slot>

            </slot>
        </div>
    </div>
</template>
<script>
    import BScroll from 'better-scroll'

export default {
    name : 'BetterScroll',
    props: {
        probeType: {
            type: Number,
            default: 0
        },
        pullUpLoad: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            bscroll: null,
        }
    },
    mounted() {
        //创建bscroll对象
        this.bscroll = new BScroll(document.querySelector(".wrapper"),{
            click: true,
            probeType: this.probeType,
            pullUpLoad: this.pullUpLoad
           
        }),
        //监听滚动位置
        this.bscroll.on('scroll', (position) => {
            this.$emit("scroll",position)
        }),
        //监听srcoll滚动到底部
        this.bscroll.on("pullingUp",() =>{
            this.$emit("pullingUp")
            })
    }
}
</script>
<style scoped>
</style>