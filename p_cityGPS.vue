<template>
  <div id="app">
    <div id="top">
      <span class="el-icon-arrow-left" @click="backcity()"></span>
      <span>{{cityGps.name}}</span>
      <span @click="backcity()">切换城市</span>
    </div>
    <div id="wrap">
      <div id="wrap_1">
        <input type="text" placeholder="输入学校、商务楼、地址" v-model="serachword" />
      </div>
      <div id="wrap_2">
        <el-button type="primary" class="btn" @click="getSearch(cityGps.id,serachword)">提交</el-button>
      </div>
    </div>
    <div id="bottom">
      <p v-show="show">搜索历史</p>
      <ul id="bottom_ul" v-show="!show">
        <li :key="i" v-for="(v,i) in serach" @click="home(v)">
          <p v-text="v.name"></p>
          <p v-text="v.address"></p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "p_cityGPS",
  data() {
    return {
      cityGps: "", //存储定位城市信息
      show: true, //是否展示搜索历史四个字
      serach: "", //存储关键字的信息
      serachword: "" //存储搜索关键字
    };
  },
  //获得城市页面传递过来的定位城市信息
  created() {
    this.cityGps = this.$route.query;
    console.log(this.cityGps);
  },
  methods: {
    //返回城市页面
    backcity() {
      this.$router.push({ name: "p_city" });
    },
    //进入主页,把选择城市的经纬度v.geohash作为参数传递下去
    home(a) {
        this.$router.push({
            name:"p_home",
            query:a
         });
    },
    //点击提交，显示城市里的地址数据--搜索地址
    getSearch(a,b){
        const api ="https://elm.cangdu.org/v1/pois?city_id=" +a+"&keyword="+b;
        this.$http({
            url: api,
            method: "get", // 请求方式
        }).then(res => {//请求数据成功之后的回调
            if (this.serachword==""){
            return;
            }
        console.log(res.data);
        this.serach = res.data;
        this.show=false;
      });

        }
  }
};
</script>

<style scoped>
#top {
  width: 3.75rem;
  height: 0.45rem;
  box-sizing: border-box;
  background-color: #3190e8;
  padding: 0.15rem 0.12rem;
  position: fixed;
  left: 0;
  top: 0;
  text-align: center;
}
#top span:nth-child(1) {
  float: left;
  color: #ffffff;
  font-size: 0.2rem;
  font-weight: 800;
}
#top span:nth-child(2) {
  color: #ffffff;
  font-size: 0.2rem;
  font-weight: 800;
  padding-left: 0.3rem;
}
#top span:nth-child(3) {
  float: right;
  color: #ffffff;
  font-size: 0.14rem;
}
#wrap {
  margin-top: 0.55rem;
  background-color: #fff;
  height: 1rem;
  padding-top: 0.05rem;
  box-sizing: border-box;
  border-top: 1px solid #e4e4e4;
  border-bottom: 2px solid #e4e4e4;
}
#wrap_1 {
  height: 0.45rem;
  text-align: center;
  padding: 0.05rem 0.18rem;
  box-sizing: border-box;
}
#wrap_1 input {
  border: 1px solid #e4e4e4;
  width: 100%;
  height: 100%;
  padding: 0.05rem 0.08rem;
  box-sizing: border-box;
}
#wrap_2 {
  height: 0.45rem;
  text-align: center;
  padding: 0.05rem 0.18rem;
  box-sizing: border-box;
}
#wrap_2 button {
  background-color: #3190e8;
  border: none;
  border-radius: 0.03rem;
  width: 100%;
  height: 100%;
  padding: 0.05rem 0.08rem;
  box-sizing: border-box;
  color: #ffffff;
  font-size: 0.16rem;
}
/* 搜索历史 */
#bottom > p {
  font-size: 0.11rem;
  padding: 0.06rem 0.1rem;
  box-sizing: border-box;
  border-bottom: 2px solid #e4e4e4;
}
#bottom_ul > li {
  width: 100%;
  height: 0.7rem;
  background-color: #fff;
  box-sizing: border-box;
  padding: 0.13rem 0.18rem;
  border-bottom: 1px solid #e4e4e4;
}
#bottom_ul > li > p:nth-child(1) {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
#bottom_ul > li > p:nth-child(2) {
  font-size: 0.12rem;
  color: #666666;
  margin-top: 0.12rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>