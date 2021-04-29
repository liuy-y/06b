<template>
  <div>
    <div class="left"><br />
      <label>用户名: <input type="text" v-model="userName" v-if="isInpShow" /><span v-else>{{userName}} </span>
      </label>
      <span v-if="isShow"  class="red">用户名长度要6-18位</span><br /><br /><br />
      <button @click="toLogin">{{login}}</button>
    </div>
    <div class="right">
      <!-- <div v-if="isAnswer"> -->
      <div class="cover" v-show="isCover"></div>
      <div class="item" v-for="(item,index) in list" :key="index">
      <!-- </div> -->
      </div>
      <!-- <div v-else> -->
          已经做完啦
          <button>再来一遍</button>
      </div>
    <!-- </div> -->

  </div>
</template>

<script>
import list from '@/assets/data1.json'
export default {
  name: "text2",
  data() {
    return {
      list,
      login: "登陆",
      isShow: false,
      userName: "",
      isInpShow: true,
      isCover: true,
      isAnswer: true,
    };
  },
  methods: {
    toLogin() {
      if (this.login == "登陆") {
        let reg = /^\w{6,18}$/;
        let flage = reg.test(this.userName);
        if (flage) {
          this.isCover = false;
          this.isInpShow = false;
          this.isShow = false;
          this.login = "退出";
        }else {
          this.isShow = true;
        }
      }else{
        this.login = "登陆";
        this.userName = '';
        this.isCover = true;
        this.isInpShow = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.left,
.right {
  width: 40%;
  height: 400px;
  border: 1px solid #ccc;
  float: left;
  .cover {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.849);
  }
  .red {
      color:red;
  }
}
</style>