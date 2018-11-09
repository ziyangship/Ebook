<template>
    <transition name="slide-right">
        <div class="content"  v-show="isShowContent">
            <div class="content-wapper" v-if="bookAviable">
                <div class="content-item" v-for="(item,index) in navigation.toc" :key="index" @click="jumpTo(item.href)">
                    <span class="label">{{item.label}}</span>
                </div>
            </div>

            <div class="content-text" v-else>
                加载中...
            </div>
        </div>
    </transition>
</template>
<script>
/*eslint-disable*/
export default {
    props:{
        isShowContent:Boolean,
        navigation:Object,
        bookAviable:Boolean
    },
    data(){
        return{

        }
    },
    methods:{
         jumpTo(href){
            this.$emit('jumpTo',href)
        },
    }
}
</script>
<style lang='scss' scoped>
@import '../assets/styles/global';
.content{
    width: 90%;
    height: 100%;
    background-color: #fff;
    position: absolute;
    overflow-y: scroll;
    left:0;
    top:0;
    z-index:102;
    .content-text{
        height: 100%;
        font-size: px2rem(16);
        @include center;
    }
    .content-item{
        width: 100%;
        border-bottom:px2rem(1) solid #eee; 
        padding-bottom: px2rem(10);
        padding-left: px2rem(20);
    .label{
        width: 80%;
        font-size: px2rem(16);
    }
    }
}
</style>