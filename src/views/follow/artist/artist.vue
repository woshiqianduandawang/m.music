<template>
  <div id="ClassifyBox">
    <div id="content">
      <div id="ClassBox">
        <div>
          <h3>推荐</h3>
          <i
            @click="jump({ type: -1, area: -1 }, 0, '发现-歌手')"
            :class="{ AcitveElement: active == 0 }"
          >
            推荐歌手
          </i>
        </div>

        <!-- 华语 -->
        <div>
          <h3>华语</h3>
          <i
            @click="jump({ type: 1, area: 7 }, 1, '华语男歌手')"
            :class="{ AcitveElement: active == 1 }"
            >男歌手</i
          >
          <i
            @click="jump({ type: 2, area: 7 }, 2, '华语女歌手')"
            :class="{ AcitveElement: active == 2 }"
            >女歌手</i
          >
          <i
            @click="jump({ type: 3, area: 7 }, 3, '华语乐队/组合')"
            :class="{ AcitveElement: active == 3 }"
            >乐队/组合</i
          >
        </div>

        <!-- 欧美 -->
        <div>
          <h3>欧美</h3>
          <i
            @click="jump({ type: 1, area: 96 }, 4, '欧美男歌手')"
            :class="{ AcitveElement: active == 4 }"
            >男歌手</i
          >
          <i
            @click="jump({ type: 2, area: 96 }, 5, '欧美女歌手')"
            :class="{ AcitveElement: active == 5 }"
            >女歌手</i
          >
          <i
            @click="jump({ type: 3, area: 96 }, 6, '欧美乐队/组合')"
            :class="{ AcitveElement: active == 6 }"
            >乐队/组合</i
          >
        </div>

        <!-- 日本 -->
        <div>
          <h3>日本</h3>
          <i
            @click="jump({ type: 1, area: 8 }, 7, '日本男歌手')"
            :class="{ AcitveElement: active == 7 }"
            >男歌手</i
          >
          <i
            @click="jump({ type: 2, area: 8 }, 8, '日本女歌手')"
            :class="{ AcitveElement: active == 8 }"
            >女歌手</i
          >
          <i
            @click="jump({ type: 3, area: 8 }, 9, '日本乐队/组合')"
            :class="{ AcitveElement: active == 9 }"
            >乐队/组合</i
          >
        </div>

        <!-- 韩国 -->
        <div>
          <h3>韩国</h3>
          <i
            @click="jump({ type: 1, area: 16 }, 10, '韩国男歌手')"
            :class="{ AcitveElement: active == 10 }"
            >男歌手</i
          >
          <i
            @click="jump({ type: 2, area: 16 }, 11, '韩国女歌手')"
            :class="{ AcitveElement: active == 11 }"
            >女歌手</i
          >
          <i
            @click="jump({ type: 3, area: 16 }, 12, '韩国乐队/组合')"
            :class="{ AcitveElement: active == 12 }"
            >乐队/组合</i
          >
        </div>

        <!-- 其他 -->
        <div>
          <h3>其他</h3>
          <i
            @click="jump({ type: 1, area: 0 }, 13, '其他男歌手')"
            :class="{ AcitveElement: active == 13 }"
            >男歌手</i
          >
          <i
            @click="jump({ type: 2, area: 0 }, 14, '其他女歌手')"
            :class="{ AcitveElement: active == 14 }"
            >女歌手</i
          >
          <i
            @click="jump({ type: 3, area: 0 }, 15, '其他乐队/组合')"
            :class="{ AcitveElement: active == 15 }"
            >乐队/组合</i
          >
        </div>
      </div>
      <div id="singers">
        <div>
          <router-link
            v-for="(item, index) in ClassSingers"
            :key="item.id"
            :to="{
              path: '/artist-page',
              query: { id: item.id },
            }"
            :class="{ 'router-link': index >= 20 }"
          >
            <li v-if="index < 20">
              <img :src="item.picUrl + '?param=130y130'" alt="" />
              <span :title="item.name">{{ SaveName(item.name) }}</span>
            </li>
            <p v-if="index >= 20">
              <i :title="item.name">{{ SaveName(item.name) }}</i>
            </p>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Artist",
  data() {
    return {
      ClassSingers: "",
      singerfund: 10,
      active: 0,
      type: -1,
      area: -1,
      title: "发现-歌手",
      unwatch: "",
    };
  },
  components: {},
  activated() {
    document.title = this.title;
    // 获取歌手数据
    this.GetClassSingers();
    this.unwatch = this.$watch("$route.query", function () {
      this.GetClassSingers();
      document.title = this.title;
    });
  },
  methods: {
    // 获取歌手数据
    GetClassSingers() {
      this.$store.state.mask = true
      this.$Request({
        url: "/artist/list",
        params: {
          type: this.$route.query.type ? this.$route.query.type : -1,
          area: this.$route.query.area ? this.$route.query.area : -1,
          limit: 99,
        },
      })
        .then(({ data: { artists: ClassSinger } }) => {
          this.ClassSingers = ClassSinger;
          this.$store.state.mask = false
        })
        .catch((arr) => {
        });
    },
    //携带数据跳转到当前页面
    jump(query, index, title) {
      this.title = title;
      this.active = index;
      this.$router.push({
        path: "/follow/artist",
        query: query,
      });
    },
  },
  computed: {
    SaveName() {
      return function (name) {
        if (name.length > 14) {
          return name.substr(0, 14) + "…";
        } else {
          return name;
        }
      };
    },
  },
  deactivated() {
    this.unwatch();
  },
};
</script>

<style scoped>
#ClassifyBox {
  position: relative;
  top: 40px;
  font-size: 0;
  font-size: 16px;
}
#content {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  padding: 20px;
  padding-top: 35px;
  border: 2px solid rgba(79, 79, 79, 0.1);
  border-top: 0;
  box-sizing: border-box;
  width: 1280px;
  background-color: #fff;
}

/* 分类按钮 */
#ClassBox {
  display: inline-block;
  position: absolute;
  top: 0px;
  left: -4px;
  border: 1px solid rgb(0, 0, 0, 0.2);
  padding: 30px;
  box-sizing: border-box;
  height: 100%;
  background-color: rgb(244, 244, 244);
}
#ClassBox div {
  margin-top: 20px;
  border-bottom: 1px solid rgb(0, 0, 0, 0.1);
}
#ClassBox div:nth-last-child(1) {
  border: 0;
}
#ClassBox h3 {
  position: relative;
  top: -6px;
  left: -8px;
}
#ClassBox i {
  display: block;
  margin-bottom: 10px;
  padding: 5px;
  font-size: 13px;
  cursor: pointer;
}
#ClassBox i:hover {
  text-decoration: underline;
}

/* 歌手 */
#singers {
  display: inline-block;
  position: relative;
  left: 200px;
  padding: 50px;
  width: 940px;
}
#singers div {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
#singers a {
  margin: 0 20px;
}
li {
  display: inline-block;
  width: 130px;
  height: 191px;
}
span {
  display: block;
  margin-top: 5px;
  width: 170px;
  text-decoration: none;
  font-size: 20px;
  overflow: hidden;
  white-space: nowrap;
  color: #000;
}
span:hover {
  text-decoration: underline;
}
img {
  display: block;
  margin: 0 auto;
  box-shadow: 0px 0px 3px rgb(0, 0, 0, 0.1);
  width: 130px;
  height: 130px;
}
.AcitveElement {
  border: 1px solid rgba(65, 65, 65, 0.1);
  border-radius: 3px;
  color: rgb(255, 0, 0);
  background-color: rgb(249, 249, 249);
}
#singers p {
  width: 10px;
  font-size: 16px;
  white-space: nowrap;
}
#singers i {
  display: block;
  width: 158px;
  overflow: hidden;
}
#singers i:hover {
  text-decoration: underline;
  color: rgb(255, 0, 0);
}
#singers .router-link {
  margin: 0 20px;
  margin-right: 157px;
  margin-top: 12px;
}
</style>