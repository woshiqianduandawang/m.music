<template>
  <div id="ArtistPage">
    <followVue></followVue>
    <div id="ArtistBox"  v-if="templateif">
      <div id="left">
        <h1>{{ SingerData.artist.name }}</h1>
        <div id="imgbox">
          <img
            v-if="imgif"
            :src="SingerData.artist.cover + '?param=640y300'"
            alt=""
          />
        </div>
        <div id="RouterBox">
          <router-link
            :to="{
              path: '/artist-page/hotsongs',
              query: {
                id: this.$route.query.id,
              },
            }"
            >热门歌曲</router-link
          >
          <router-link
            :to="{
              path: '/artist-page/album',
              query: {
                id: this.$route.query.id,
              },
            }"
            >所有专辑</router-link
          >
          <router-link
            :to="{
              path: '/artist-page/mv',
              query: {
                id: this.$route.query.id,
              },
            }"
            >相关mv</router-link
          >
          <router-link
            :to="{
              path: '/artist-page/desc',
              query: {
                id: this.$route.query.id,
              },
            }"
            >艺人介绍</router-link
          >
        </div>
        <router-view name="artist"></router-view>
      </div>
    </div>
  </div>
</template>

<script>
import followVue from "@/views/follow/follow.vue";
export default {
  name: "ArtistPage",
  components: {
    followVue,
  },
  data() {
    return {
      SingerData: "",
      templateif: false,
      imgif: false,
    };
  },
  methods: {
  },
  activated() {
    this.$store.state.mask = true
    // 获取歌手信息
    this.$Request({
      url: "/artist/detail",
      params: {
        id: this.$route.query.id,
      },
    })
      .then(({ data: { data: a } }) => {
        this.SingerData = a;
        document.title = a.artist.name + " " + a.identify.imageDesc;
        this.imgif = true;
        this.templateif = true;
      })
      .catch((arr) => {
      });
  },
  deactivated() {
    this.imgif = false;
    this.templateif = false;
  },
};
</script>

<style scoped>
#ArtistPage {
  position: relative;
  padding-bottom: 120px;
}
#ArtistBox {
  position: relative;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  padding: 30px;
  padding-top: 0px;
  width: 1280px;
  box-sizing: border-box;
  background-color: #fff;
}
#left{
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  padding: 30px;
  width: 960px;
}
h1 {
  position: relative;
  top: 10px;
  left: 28px;
  margin: 0;
  font-size: 6vw;
}
#imgbox {
  padding-top: 20px;
}
img {
  display: block;
  margin: 0 auto;
  width: 960px;
  height: 450px;
  text-align: center;
}
#RouterBox {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  border: 1.5px solid rgba(67, 67, 67, 0.3);
  width: 960px;
  height: 51px;
  line-height: 50px;
  box-sizing: border-box;
  font-size: 0;
  background-color: rgb(242, 242, 242);
}
a {
  display: inline-block;
  width: 200px;
  height: 47px;
  text-align: center;
  font-size: 1.25vw;
  font-weight: 700;
  box-sizing: border-box;
  background-color: rgb(242, 242, 242);
}

.active {
  border: 1.5px solid rgba(67, 67, 67, 0.3);
  border-bottom: 0;
  border-top: 2px solid rgb(255, 0, 0);
  background-color: rgb(255, 255, 255);
  height: 50px;
}
a:nth-child(1) {
  border-left: 0;
}
a:nth-child(4) {
  border-right: 0;
  width: 160px;
}
</style>