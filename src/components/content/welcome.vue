<template>
  <!-- 欢迎弹窗组件 -->
  <div ref="welcome" id="welcome" v-if="show">
    <div>
      <span>Welcome</span>
      <p>欢迎莅临小站！</p>
      <button @click="close">关闭</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Welcome",
  data() {
    return {
      show: true,
      count: 3,
    };
  },
  methods: {
    close() {
      if (this.count == 0) {
        this.show = false;
      } else {
        alert(`关闭失败嘻嘻嘻，再点${this.count}次试试`);
        this.count--;
      }
    },
  },
  mounted() {
    // 加载中的遮罩层
    this.$refs.welcome.style.width = window.innerWidth + "px";
    this.$refs.welcome.style.height = window.innerHeight + 65 + "px";
    this.$refs.welcome.style.top = -65 + "px";
    window.addEventListener("resize", () => {
      this.$refs.welcome.style.width = window.innerWidth + "px";
      this.$refs.welcome.style.height = window.innerHeight + 65 + "px";
    });
    window.addEventListener("scroll", () => {
      this.$refs.welcome.style.top = window.scrollY - 65 + "px";
    });
  },
};
</script>

<style scoped>
#welcome {
  position: absolute;
  left: 0;
  top: 0px;
  z-index: 999;
  background-color: rgb(0, 0, 0, 0.5);
}
#welcome div {
  position: relative;
  top: 50%;
  left: 50%;
  border: 3px solid rgb(0, 0, 0, 0.3);
  width: 100%;
  height: 400px;
  transform: translate(-50%, -50%);
  font-size: 60px;
  color: #fff;
  background-color: rgb(120, 139, 62);
}
#welcome div span {
  position: absolute;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
  font-weight: 700;
}
#welcome div p {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 93%;
  transform: translate(-50%, -50%);
}
#welcome button {
  position: absolute;
  bottom: 3px;
  left: 50%;
  transform: translateX(-50%);
  width: 300px;
  height: 90px;
}
</style>