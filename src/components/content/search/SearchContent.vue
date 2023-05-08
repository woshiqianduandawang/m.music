<template>
  <!-- 搜索结果页面 -->
  <div id="SearchContent">
    <followVue></followVue>
    <div v-if="searchif" id="content">
      <p>以下是搜索{{ this.$route.query.content }}的结果：</p>
      <!-- 遍历搜索到的歌手 -->
      <router-link
        id="SearchSingerBox"
        :to="{
          path: '/artist-page',
          query: {
            id: singer.id,
          },
        }"
        v-show="SingerShow"
      >
        <img :src="singer.img1v1Url" alt="" />
        <span>{{ singer.name }}</span>
      </router-link>
      <table>
        <tr>
          <th></th>
          <th><i>歌曲</i></th>
          <th><i>时长</i></th>
          <th><i>歌手</i></th>
          <th><i>专辑</i></th>
        </tr>
        <tr v-for="(item, index) in songs" :key="index" id="tr">
          <!-- 序号 -->
          <td>
            {{ index + 1 }}
          </td>
          <!-- 歌曲名 -->
          <td>
            <span
              :title="item.name"
              @click="
                $store.commit('PlayMusic', { songs: songs, index: index })
              "
              >{{ OmitName(item.name, 16) }}</span
            >
          </td>

          <td>
            <!-- 歌曲时长 -->
            <i>
              {{ GetTime(item.dt / 1000 / 60) }}:
              {{ GetTime((item.dt / 1000) % 60) }}
            </i>
          </td>
          <td>
            <!-- 拼接歌手名(如果有多个创作者) -->
            <div v-for="(item2, inx) in item.ar" :key="inx">
              <span @click="jump(item2.id)" :title="GetArName(item.ar)">
                {{ item2.name }}
              </span>
              <strong v-if="inx < item.ar.length - 1">/</strong>
            </div>
          </td>
          <!-- 所属专辑 -->
          <td>
            <span :title="item.al.name">{{ OmitName(item.al.name, 11) }}</span>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import mixincomputed from "@/common/mixin-computed";
import followVue from "@/views/follow/follow.vue";
export default {
  name: "SearchContent",
  components: {
    followVue,
  },
  mixins: [mixincomputed],
  data() {
    return {
      songs: "", //搜索到的歌曲
      singer: "", //搜索到的歌手
      searchif: true, //是否重新渲染页面
      SingerShow: false, //是否显示搜索到的歌手
    };
  },
  activated() {
    this.singer = "";
    this.SingerShow = false;
    this.searchif = false;
    // 搜索结果-歌曲
    this.$Request({
      url: "/cloudsearch",
      params: {
        keywords: this.$route.query.content,
      },
    })
      .then(
        ({
          data: {
            result: { songs: a },
          },
        }) => {
          this.songs = a;
          this.searchif = true;
          this.$store.state.mask = false
          // console.log(a);
        }
      )
      .catch((arr) => {
      });
    // 搜索结果-多重匹配
    this.$Request({
      url: "/search/multimatch",
      params: {
        keywords: this.$route.query.content,
      },
    })
      .then(({ data: { result: a } }) => {
        if (a.artist) {
          this.SingerShow = true;
          this.singer = a.artist[0];
        }
      })
      .catch((arr) => {
      });
  },
  methods: {},
};
</script>

<style scoped>
#SearchContent {
  position: relative;
}
#content {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 1280px;
  font-size: 16px;
}
#SearchSingerBox {
  display: block;
  padding-top: 20px;
  width: 150px;
  height: 150px;
  text-align: center;
}
#SearchSingerBox span {
  display: block;
}
#SearchSingerBox span:hover {
  text-decoration: underline;
}
#SearchSingerBox img {
  border-radius: 50px;
  width: 100px;
  height: 100px;
  text-align: center;
}
/* 歌曲 */
table {
  display: block;
  position: relative;
  top: 50px;
  left: 50px;
  border: 1px solid rgb(0, 0, 0, 0.1);
  border-top: 0;
  border-collapse: collapse;
  width: 1150px;
  background-color: #fff;
}
tr,
th {
  height: 100px;
  line-height: 100px;
  font-size: 16px;
}
tr:nth-child(2n) {
  background-color: rgb(245, 244, 244);
}
#tr:hover {
  background-color: rgb(208, 208, 208);
}
td:nth-child(4) {
  padding-left: 20px;
  width: 234px;
}
th:nth-child(4) {
  width: 254px;
}
th {
  box-shadow: 0px 2px 5px rgb(0, 0, 0, 0.1);
  /* height: 42px;
    line-height: 42px; */
}
table i {
  float: left;
  padding-left: 20px;
}
th,
td {
  display: inline-block;
  width: 325px;
  white-space: nowrap;
  overflow: hidden;
}
th {
  border: 1px solid rgb(0, 0, 0, 0.1);
  border-right: 0;
  box-sizing: border-box;
}
th:nth-child(1) {
  border-left: 0;
  padding-left: 100px;
  width: 50px;
}
td:nth-child(1) {
  padding-left: 20px;
  width: 80px;
}
td:nth-child(2) {
  padding-left: 20px;
  width: 305px;
}
th:nth-child(3),
td:nth-child(3) {
  width: 141px;
}
td:nth-child(5) {
  padding-left: 20px;
  width: 305px;
}
table p {
  padding-left: 20px;
  width: 0;
  height: 40px;
  line-height: 40px;
  font-size: 20px;
  cursor: pointer;
}
table span:hover,
table p:hover {
  color: #fff;
}
table div {
  display: inline-block;
}
table span {
  display: inline-block;
  height: 40px;
  line-height: 40px;
  font-size: 20px;
  cursor: pointer;
}
td strong {
  font-size: 20px;
  font-weight: 400;
}
</style>