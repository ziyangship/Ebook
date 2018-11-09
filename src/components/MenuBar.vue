<template>
    <div class="menu-bar">
        <transition name="slide-up">
            <div class="menu-wrapper" :class="{'hide-box-shadow':fontSet || !TitleAndMenuShow}" v-show="TitleAndMenuShow">
                <div class="icon-wrapper">
                    <span class="icon-cloud icon" @click="showTheme(3)"></span>
                </div>
                <div class="icon-wrapper" @click="showTheme(2)">
                    <span class="icon" >进度</span>
                </div>
                <div class="icon-wrapper" @click="showTheme(1)">
                    <span class="icon">主题</span>
                </div>
                <div class="icon-wrapper" @click="fontSeting(0)">
                    <span class=" icon">字体</span>
                </div>
            </div>
        </transition>
        <transition name="slide-up">
            <div class="setting-wraper" v-show="fontSet">
                <div class="setting-fontsize" v-if="showTag === 0">
                    <div class="preview" :style="{'font-size':fontSizeList[0].fontSize+'px'}">A</div>
                    <div class="select">
                        <div class="select-wapper" v-for="(item,index) in fontSizeList" :key="index" @click="setFontSize(item.fontSize)">
                        <div class="line"></div>
                        <div class="point-wapper">
                            <div class="point" v-show="defaultFontsize === item.fontSize">
                                <div class="small-point"></div>
                            </div>
                        </div>
                        <div class="line"></div>
                    </div>
                    </div>
                    <div class="preview" :style="{'font-size':fontSizeList[fontSizeList.length-1].fontSize+'px'}">A</div>
                </div>
                <div class="setting-theme" v-else-if="showTag === 1">
                    <div class="setting-theme-item" v-for="(item,index) in themeList" :key="index" @click="setTheme(index)"> 
                        <div class="setting-preview" :style="{background:item.style.body.background}"></div>
                        <div class="theme-name" :class="{'selected':index===defaultTheme}">{{item.name}}</div>
                    </div>
                </div>
                <div class="setting-progress" v-else-if="showTag === 2">
                    <div class="progress-wapper">
                        <input class="progress" 
                        type="range" 
                        max="100" 
                        min="0" 
                        step="1"
                        @change="onProgressChange($event.target.value)"
                        @input ="onProgressInput($event.target.value)"
                        :value="progress"
                        :disabled="!bookAviable"
                        ref="progress"
                        >
                        <div class="text-wapper">
                        {{bookAviable ? progress +'%': '加载中...'}}
                    </div>
                    </div>
                </div>
            </div>
        </transition>
         <Content-view 
                :isShowContent="isShowContent"
                :navigation="navigation"
                :bookAviable="bookAviable"
                @jumpTo="jumpTo"
                ></Content-view>
                <transition name=fade>
                    <div class="content-mask" v-show="isShowContent" @click="hideContent"></div>
                </transition>
    </div>
</template>
<script>
/*eslint-disable */
/*eslint-disable-next-line */
import ContentView from "@/components/Content";
export default {
    components:{
        ContentView
    },
  props: {
    TitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: Array,
    defaultFontsize:Number,
    themeList:Array,
    defaultTheme:Number,
    navigation:Object,
    bookAviable:Boolean
  },
  data() {
    return {
      fontSet: false,
      showTag:0,
      progress:0,
      isShowContent:false
    };
  },
  methods: {
    fontSeting(tag) {
      this.fontSet = true;
      this.showTag = tag;
    },
    //目录跳转
    jumpTo(href){
        //调用父级jumpTo方法
        this.$emit('jumpTo',href)
    },
    hideContent(){
        this.isShowContent = false
    },
    //显示主题切换框
    showTheme(tag){
        this.showTag = tag;
        if(this.showTag === 3){
            this.fontSet = false;
            this.isShowContent = true;
        }else{
            this.fontSet = true;
        }
    },
    hideSetting() {
      this.fontSet = false;
    },
    //设置电子书字体大小
    setFontSize(fontSize){
        this.$emit('setFontSize',fontSize)
    },
    setTheme(index){
        this.$emit('selectTheme',index)
    },
    onProgressInput(progress){
        this.progress = progress;
        console.log(this.progress)
        this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    onProgressChange(progress){
        this.$emit('onProgressChange',progress)
    }
  }
};
</script>
<style lang='scss' scoped>
@import "../assets/styles/global";
.menu-bar {
  .setting-wraper {
    position: absolute;
    bottom: px2rem(48);
    left: 0;
    width: 100%;
    height: px2rem(60);
    background-color: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .setting-fontsize {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select{
          display: flex;
          flex: 1;
        .select-wapper {
        flex: 1;
        display: flex;
        align-items: center;
        &:first-child{
            .line{
                &:first-child{
                    border-top: none;
                }
            }
        }
        &:last-child{
            .line{
                &:last-child{
                    border-top:none; 
                }
            }
        }
        .line {
          flex: 1;
          height: 0;
          border-top: px2rem(1) solid #ccc;
        }
        .point-wapper {
          flex: 0 0 0;
          position: relative;
          width: 0;
          height: px2rem(7);
          border-left: px2rem(1) solid #ccc;
          .point{
              position: absolute;
              width: px2rem(20);
              height: px2rem(20);
              background-color: white;
              top:px2rem(-8);
              left: px2rem(-10);
              box-shadow: 0 px2rem(4) px2rem(4) rgba($color: #000000, $alpha: 0.15);
              border-radius: 50%;
              border:px2rem(1) solid #ccc;  
              @include center; 
              .small-point{
                  width: px2rem(5);
                  height: px2rem(5);
                  background: #000000;
                  border-radius: 50%;
              }
          }
        }
      }
      }
    }
    .setting-theme{
        height: 100%;
        display: flex;
        .setting-theme-item{
            flex: 1;
            display: flex;
            flex-direction: column;
            padding:px2rem(5);
            box-sizing: border-box;
            .setting-preview{
                flex: 1;
                border:px2rem(1) solid #eee;
                box-sizing: border-box;
            }
            .theme-name{
                flex: 0 0 px2rem(20);
                font-size: px2rem(14);
                color: #eee;
                @include center;
                &.selected{
                    color: #000
                }
            }
        }
    }
    .setting-progress{
        position: relative;
        width: 100%;
        height: 100%;
       .progress-wapper{
           display: flex;
           flex-direction: column;
           width: 100%;
           @include center;
           padding: 0 px2rem(20);
           box-sizing: border-box;
           height: 100%;
           .text-wapper{
               font-size: px2rem(14);
           }
           .progress{
               width: 100%;
               -webkit-appearance: none;
               height: px2rem(2);
               margin:px2rem(20) 0;
               background: -webkit-linear-gradient(#999,#999) no-repeat, #ddd;
               background-size: 0% 100%; 
           }
           &:focus{
               outline: none;
           }
           ::-webkit-slider-thumb{
               -webkit-appearance: none;
               height: px2rem(20);
               width: px2rem(20);
               border-radius: 50%;
               background: white;
               box-shadow: 0 4px 4px 0 rgba(0,0,0,0.15);
               border:px2rem(1) solid #ddd;
           }
       } 
    }
  }
  .content-mask{
      position: absolute;
      left: 0;
      top:0;
      width: 100%;
      height: 100%;
      display: flex;
      z-index: 101;
      background-color: rgba(51,51,51,0.8)
  }
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    display: flex;
    left: 0;
    width: 100%;
    z-index: 101;
    background-color: #fff;
    height: px2rem(48);
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    justify-content: center;
    &.hide-box-shadow {
      box-shadow: none;
    }
    .icon-wrapper {
      flex: 1;
      @include center;
    }
  }
}
</style>