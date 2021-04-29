<template>
  <div class="home">
    <button @click="close">{{dis}} </button><br>
    所在城市
    <select v-model="city" :disabled="isDis">
      <option value="-0">请选择城市</option>
      <option :value=item.name v-for="item in this.cityData" :key="item.id">{{item.name}} </option>
    </select><br>
    用户名：<input type="text" v-model="myname" :disabled="isDis"><br>
    年龄： <input type="number" v-model="age" :disabled="isDis"> <br>
    <button @click="appendtab" :disabled="isDis">{{add}}</button>
    <!-- 筛选 -->
    搜索 <input type="text" @keyup.enter="sear" v-model="search">
    <!-- 表格 -->
    <table>
      <thead>
        <tr>
          <th></th>
          <th>id</th>
          <th>姓名</th>
          <th>年龄
            <button @click="agesort">排序</button>
          </th>
          <th>城市
            <button @click="citySort">排序</button>
          </th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,index) in this.myList" :key="item.id">
          <td>
            <input type="checkbox" v-model="checkedArr" :value="index" :disabled="isDis">
          </td>
          <td>{{item.id}} </td>
          <td>{{item.name}} </td>
          <td>{{item.age}} </td>
          <td>{{item.city}} </td>
          <td>
            <button @click="change(index)" :disabled="isDis">修改</button>
            <button @click="del(item)" :disabled="isDis">删除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="delsel" :disabled="isDis">选中删除</button>
  </div>
</template>

<script>
import cityData from "@/assets/data.json";
export default {
  name: "home",
  data() {
    return {
      dis: "锁定",
      add: "提交",
      cityData,
      myname: "",
      city: "",
      age: 0,
      search:'',
      myList: [], //全部数组
      checkedArr: [], //选中数组
      isDis: false, //禁用
      currentIndex: 0,
      time: 4, //定时器
    };
  },
  methods: {
    // 年龄排序
    agesort(){
      this.myList.sort(function (a, b){
        let age = a.age
        let bge = b.age
        return age - bge
      })
    },
    // 城市排序
    citySort(){
      this.myList.sort(function (a,b) {
        let acity = a.city
        let bcity = b.city
        return acity.localeCompare(bcity)
      })
    },
    // 搜索
    sear(){
      let arr = this.myList.filter(item=>{
        return item.age ? true :item.includes(this.search)
        &&(item.city ? true :item.includes(this.search))
        &&(item.name ? true :item.includes(this.search))
      })
      console.log(arr);
      this.myList = arr
    },
    // 选中删除
    delsel() {
      this.checkedArr
        .sort(function (a, b) {
          return b - a;
        })
        .forEach((item) => {
          this.myList.splice(item, 1);
        });
      this.checkedArr = [];
    },
    // 修改
    change(index) {
      let data = this.myList[index];
      // console.log(data);
      this.myname = data.name;
      this.age = data.age;
      this.city = data.city;
      this.add = "确认修改";
      this.currentIndex = index;
    },
    // 增加
    appendtab() {
      let obj = {
        id: new Date().getTime(),
        name: this.myname,
        age: this.age,
        city: this.city,
      };
      if (this.add == "提交") {
        this.myList.push(obj);
      } else {
        obj.id = this.myList[this.currentIndex].id;
        this.myList.splice(this.currentIndex, 1, obj);
        this.add = "提交";
      }

      this.city = "";
      this.age = "";
      this.myname = "";
    },
    // 删除
    del(item) {
      this.myList.forEach((arritem, index) => {
        if (arritem.id === item.id) {
          console.log(index);
          this.myList.splice(index, 1);
        }
      });
    },
    // 禁用
    close() {
      if (this.dis == "锁定") {
        this.isDis = true;
        this.dis = '解锁'

      } else {
        let timer = setInterval(() => {
          this.time--;
          this.dis = this.time + "秒后解锁";
          if(this.time == 0){
            this.dis == "锁定"

            clearInterval(timer)
            this.isDis = false;
            this.time = 3
          }
        },1000);

      }
    },
  },
};
</script>

<style lang="scss" scoped>
input {
  margin: 10px;
}
table {
  border: 1px solid #ccc;

}
td,th {
  padding: 5px 10px;
  border: 1px solid #ccc;
}
tr:nth-child(odd) {
  background-color: #fff;
}
tr:nth-child(odd) {
  background-color: rgb(255, 208, 0);
}
th {
  background-color: #fff;
}
</style>