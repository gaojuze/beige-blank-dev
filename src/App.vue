<template>
  <div id="app">
      <router-view class="router-view"/>
    <pop-up  v-model="showQrcode" style="text-align: center">
      <img class="qrcode" src="./assets/qrcode.png" alt="qrcode">
      <div class="slogan">可微信扫描二维码体验👆<br/>点击图片外区域可关闭二维码✨</div>
    </pop-up>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import {Component} from "vue-property-decorator";
import PopUp from './views/PopUp.vue'
window.onload = function () {
  document.addEventListener('touchstart', function (event) {
    if (event.touches.length > 1) {
      event.preventDefault();
    }
  }, {
    passive: false  // 关闭被动监听
  });
  let lastTouchEnd = 0;
  document.addEventListener('touchend', function (event) {
    let now = (new Date()).getTime();
    if (now - lastTouchEnd <= 300) {
      event.preventDefault();
    }
    lastTouchEnd = now;
  }, false);
};
@Component({
  components: {PopUp}
})
export default class App extends Vue {
  transitionName = ''
  showQrcode = false

  created(){
    if(document.documentElement.clientWidth >500){
      this.showQrcode = true
    }
  }
}
</script>
<style lang="scss">
@import "~@/assets/style/helper.scss";
@import "~@/assets/style/reset.scss";
@import "~@/assets/style/animation.scss";
body {
  max-width: 500px;
  margin: 0 auto;
}

#app {
  position: relative;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100%;
}

.router-view {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.slogan{
  color: black;
  font-size: larger;
  font-weight: bolder;
  background: lighten($color-highlight,5%)

}

</style>
