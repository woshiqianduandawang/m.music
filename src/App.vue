<template>
  <div id="app">
    <Hade></Hade>
    <keep-alive>
      <router-view />
    </keep-alive>
    <Player v-if="$store.state.playerif"></Player>
    <Welcome></Welcome>
  </div>
</template>

<script>
import Hade from "./components/content/head.vue";
import Player from "@/components/content/player";
import Welcome from "@/components/content/welcome";
// if( /Android|webOS|iPhone|iPad|iPod|BlackBerry/i.test(navigator.userAgent) ) {
// // 执行相应代码或直接跳转到手机页面
// alert('手机')
// } else {
// alert('桌面')
// // 执行桌面端代码
// }

export default {
  name: "App",
  components: {
    Hade,
    Player,
    Welcome
  },
};
</script>

<style>
@import url(./assets/css/base.css);
#app {
  box-shadow: 1px 1px 5px rgb(0, 0, 0, 0.3);
}

</style>
