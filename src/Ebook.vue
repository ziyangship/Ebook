<template>
    <div class="ebook">
        <Title-bar :TitleAndMenuShow="TitleAndMenuShow"></Title-bar>
        <div class="read-wrapper">
           <div id="read"></div>
            <div class="mask">
                <div class="left" @click="prevPage"></div>
                <div class="center" @click="toggleMenu"></div>
                <div class="right" @click="nextPage"></div>
            </div>
        </div>
        <Menu-bar 
        :TitleAndMenuShow="TitleAndMenuShow" 
        ref="menuBar" 
        :fontSizeList="fontSizeList" 
        :defaultFontsize="defaultFontsize" 
        :defaultTheme="defaultTheme" 
        :themeList="themeList" 
        :bookAviable="bookAviable"
        :navigation="navigation"
        @selectTheme="selectTheme"
        @onProgressChange="onProgressChange"
        @jumpTo="jumpTo"
        @setFontSize="setFontSize"></Menu-bar>
    </div>
</template>
<script>
/* eslint-disable */
import Epub from "epubjs";
import { log } from "util";
import TitleBar from "@/components/TitleBar";
import MenuBar from "@/components/MenuBar";
const DOWNLOAD_URL = "/static/204151.epub";
global.ePub = Epub;
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      TitleAndMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontsize: 16,
      defaultTheme:0,
      themes:{},
      bookAviable:false,
      themeList:[
          {
              name:'default',
              style:{
                  body:{
                      'color':'#000',
                      'background':'#fff',
                  }
              }
          },
          {
              name:'eye',
              style:{
                  body:{
                      'color':'#000',
                      'background':'#ceeaba',
                  }
              }
          },
          {
              name:'night',
              style:{
                  body:{
                      'color':'#fff',
                      'background':'#000',
                  }
              }
          },
          {
              name:'gold',
              style:{
                  body:{
                      'color':'#000',
                      'background':'rgb(241,236,236)',
                  }
              }
          }
      ]
    }
  },
  methods: {
    showEpub() {
      //生成book
      this.book = new Epub(DOWNLOAD_URL);
      //生成rendition,通过Book.renderTo
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      this.rendition.display();
      //获取themes对象
      this.themes = this.rendition.themes;
      this.setFontSize(this.defaultFontsize);
      this.registerTheme();
      this.selectTheme(this.defaultTheme);
      //获取location对象
      this.book.ready.then(()=>{
        return this.book.locations.generate()
        return this.book.navigation.generate()
      }).then(result=>{
          this.navigation = this.book.navigation;
          this.locations = this.book.locations;
          this.bookAviable = true;
      })
    },
    jumpTo(href){
        this.rendition.display(href);
        this.hideMenuAndContent()
    },
    hideMenuAndContent(){
        this.TitleAndMenuShow = false;
        this.$refs.menuBar.hideSetting(); 
        this.$refs.menuBar.hideContent();
    },
    onProgressChange(progress){
        const percentage = progress / 100; 
        const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0;
        this.rendition.display(location);
    },
    registerTheme(){
        this.themeList.forEach(theme=>{
            this.themes.register(theme.name,theme.style)
        })
    },
    selectTheme(index){
        this.themes.select(this.themeList[index].name);
        this.defaultTheme = index;
    },
    toggleMenu() {
      this.TitleAndMenuShow = !this.TitleAndMenuShow;
      var self = this;
      if (!self.TitleAndMenuShow) {
        self.$refs.menuBar.hideSetting();
      }
    },
    prevPage() {
      if (this.rendition) {
        this.rendition.prev();
      }
    },
    setFontSize(fontSize){
        this.defaultFontsize = fontSize;
        if(this.themes){
            this.themes.fontSize(fontSize + 'px')
        }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next();
      }
    }
  },
  mounted() {
    this.showEpub();
  }
};
</script>
<style lang='scss' scoped>
@import "/assets/styles/global";
.ebook {
  position: relative;
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      display: flex;
      width: 100%;
      height: 100%;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>