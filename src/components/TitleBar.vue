<template>
    <transition name="slide-down">
        <div class="title-wrapper" v-show="TitleAndMenuShow">
            <div class="left">
                <span class="icon-close icon"></span>
            </div>
            <div class="right">
                <div class="icon-wrapper">
                    <span class="icon-cloud icon"></span>
                </div>
                <div class="icon-wrapper">
                    <span class="icon-cloud-upload icon"></span>
                </div>
                <div class="icon-wrapper">
                    <span class="icon-user-solid-square icon"></span>
                </div>
            </div>
        </div>
    </transition>
</template>
<script>
/*eslint-disable*/
export default{
    props:{
       TitleAndMenuShow:{
           type:Boolean,
           default:false
       } 
    }
}
</script>
<style lang='scss' scoped>
@import '../assets/styles/global';
.title-wrapper{
            position: absolute;
            top:0;
            left: 0;
            width: 100%;
            z-index: 101;
            display: flex;
            background-color: #fff;
            height: px2rem(48);
            box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, 0.15);
            .left{
                flex:0 0 px2rem(60);
                @include center;
            }
            .right{
                flex:1;
                display: flex;
                justify-content: flex-end;
                .icon-wrapper{
                    flex: 0 0 px2rem(40);
                     @include center;
                }
            }
            &.slide-down-enter,&.slide-down-leave-to{
                transform: translate3d(0,-100%,0)
            }
            &.slide-down-enter-to,&.slide-down-leave{
                transform: translate3d(0,0,0)
            }
            &.slide-down-enter-active,&.slide-down-leave-active{
                transition: all .3s linear;
            }
        }
</style>