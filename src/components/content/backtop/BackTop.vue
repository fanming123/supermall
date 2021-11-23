<template>
    <div class="backtop">
        <img src="@/assets/img/home/backtop.svg" alt="">
    </div>
</template>
<script>
export default {
    name: 'BackTop'
}
</script>
<style scoped>
    .backtop {
        position: fixed;
        bottom: 44px;
        right: 0;      

    }
    .backtop img {
        width: 50px;
        height: 50px;
    }
    
</style>