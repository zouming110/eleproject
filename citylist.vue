<template>
  <div id="city">
    <!-- 顶部导航栏 -->
    <div id="city_nav">
      <span class="city_nav_left">ele.me</span>
      <!-- <span class="el-icon-user city_nav_right"></span> -->
      <span class="city_nav_right">登录|注册</span>
    </div>
    <!-- 当前定位城市 -->
    <div id="city_GPS">
      <div class="city_GPS_lin"></div>
      <div class="city_GPS_lin1">
        <span class="city_GPS_lin1_left">当前定位城市:</span>
        <span class="city_GPS_lin1_right">定位不准时，请在城市列表中选择</span>
      </div>
      <div class="city_GPS_lin2" @click="city_GPSBtn(cityGPS)">
        <span class="city_GPS_lin2_left" v-show="show">{{cityGPS.name}}</span>
        <span class="el-icon-arrow-right city_GPS_lin2_right"></span>
      </div>
    </div>
    <!-- 热门城市 -->
    <div id="city_hot">
      <div class="city_hot_h">
        <span>热门城市</span>
      </div>
      <ul class="city_hot_ul1">
        <li v-for="(v,i) in cityHot" :key="i" v-show="show" @click="city_GPSBtn(v)">
          <span>{{v.name}}</span>
        </li>
      </ul>
    </div>
    <!-- 所有城市 -->
    <div id="city_all">
      <div class="city_all_div" v-for="(v,i) in cityAllSS" :key="i" v-show="show">
        <p class="city_all_div_title">
          {{v}}
          <span class="city_all_div_span" v-if="i==0">(按字母排序)</span>
        </p>
        <ul>
          <li v-for="(v1,i1) in cityAll[v]" :key="i1" v-show="show" @click="city_GPSBtn(v1)">{{v1.name}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "citylist",
  data() {
    return {
      show: false,
      cityGPS: {},
      city:"",
      cityHot: [],
      cityAll: [], //得到的所有城市对象
      cityAllS: [], //得到所有城市对象中的key
      cityAllSS: [] //对所有key进行升序排序
    };
  },
  created() {
    this.getCityGPS();
    this.getCityHot();
    this.getCityAll();
  },
  methods: {
    // get请求方式请求定位城市
    getCityGPS() {
      const api = "https://elm.cangdu.org/v1/cities?type=guess";
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        console.log(res);
        this.cityGPS = res.data;
        this.show = true;
        this.$nextTick(() => {});
      });
    },
    // get请求方式请求热门城市
    getCityHot() {
      const api = "https://elm.cangdu.org/v1/cities?type=hot";
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        console.log(res);
        this.cityHot = res.data;
        this.show = true;
        this.$nextTick(() => {});
      });
    },
    // get请求方式请求所有城市
    getCityAll() {
      const api = "https://elm.cangdu.org/v1/cities?type=group";
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        this.cityAll = res.data;
        console.log(this.cityAll);
        for (var k in this.cityAll) {
          this.$set(this.cityAllS, this.cityAllS.length, k);
        }
        console.log(this.cityAllS);
        this.cityAllSS = this.cityAllS.sort();
        console.log(this.cityAllSS);
        this.show = true;
        this.$nextTick(() => {});
      });
    },
    // 跳转定位城市
    city_GPSBtn(a) {
      const api = "https://elm.cangdu.org/v1/cities/"+a.id;
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        // 编程式跳转
        this.$router.push({
        name: "cityGPS_search",
        query: res.data
      });
      });
    }
  }
};
</script>

<style scoped>
#city {
  overflow: auto;
}
/* 顶部导航栏 */
#city_nav {
  width: 3.75rem;
  height: 0.45rem;
  box-sizing: border-box;
  background-color: #3190e8;
  padding: 0.15rem 0.12rem;
  position: fixed;
  left: 0;
  top: 0;
}
#city_nav > .city_nav_left {
  float: left;
  color: #ffffff;
}
#city_nav > .city_nav_right {
  float: right;
  color: #ffffff;
  font-size: 0.15rem;
}

/* 定位城市 */
#city_GPS {
  width: 3.75rem;
  height: 1.35rem;
  box-sizing: border-box;
  background-color: #ffffff;
  /* margin-top: 0.45rem; */
}
.city_GPS_lin {
  width: 100%;
  height: 0.45rem;
}
/* 第一行 */
.city_GPS_lin1 {
  width: 100%;
  height: 0.45rem;
  box-sizing: border-box;
  line-height: 0.45rem;
  border-bottom: 1px solid #e4e4e4;
  padding: 0 0.1rem;
}
.city_GPS_lin1_left {
  font-size: 0.12rem;
  float: left;
  color: #666666;
}
.city_GPS_lin1_right {
  font-size: 0.11rem;
  float: right;
  color: #999999;
  font-weight: 700;
}
/* 第二行 */
.city_GPS_lin2 {
  width: 100%;
  height: 0.45rem;
  box-sizing: border-box;
  line-height: 0.45rem;
  border-bottom: 2px solid #e4e4e4;
  padding: 0 0.1rem;
}
.city_GPS_lin2_left {
  font-size: 0.2rem;
  float: left;
  color: #3190e8;
}
.city_GPS_lin2_right {
  font-size: 0.2rem;
  float: right;
  color: #999999;
  font-weight: 800;
  margin-top: 0.12rem;
}

/* 热门城市 */
#city_hot {
  width: 3.75rem;
  /* height: 1.25rem; */
  box-sizing: border-box;
  background-color: #ffffff;
  margin-top: 0.1rem;
  border-top: 2px solid #e4e4e4;
  border-bottom: 1px solid #e4e4e4;
}
.city_hot_h {
  width: 100%;
  height: 0.35rem;
  box-sizing: border-box;
  line-height: 0.35rem;
  border-bottom: 1px solid #e4e4e4;
  padding: 0 0.1rem;
}
.city_hot_h > span {
  font-size: 0.13rem;
  color: #666666;
}
.city_hot_ul1 {
  width: 100%;
  height: 0.9rem;
  box-sizing: border-box;
  /* padding: 0 0.1rem; */
}
.city_hot_ul1 > li {
  list-style: none;
  border-right: 1px solid #e4e4e4;
  border-bottom: 1px solid #e4e4e4;
  float: left;
  box-sizing: border-box;
  width: 0.9375rem;
  text-align: center;
  height: 0.45rem;
  line-height: 0.45rem;
  font-size: 0.14rem;
  color: #3190e8;
}

/* 所有城市 */
#city_all {
  width: 3.75rem;
  /* height: 1.25rem; */
  box-sizing: border-box;
  /* background-color: #ffffff; */
  margin-top: 0.1rem;
}
.city_all_div {
  width: 3.75rem;
  /* height: 1.5rem; */
  box-sizing: border-box;
  background-color: #ffffff;
  margin-top: 0.1rem;
  border-top: 1px solid #e4e4e4;
  border-bottom: 1px solid #e4e4e4;
}
.city_all_div_title {
  width: 100%;
  height: 0.35rem;
  box-sizing: border-box;
  line-height: 0.35rem;
  border-bottom: 1px solid #e4e4e4;
  padding: 0 0.1rem;
  font-size: 0.15rem;
  color: #666666;
}
.city_all_div_span {
  width: 100%;
  height: 0.35rem;
  box-sizing: border-box;
  line-height: 0.35rem;
  padding: 0 0.1rem;
  font-size: 0.12rem;
  color: #999999;
}
.city_all_div > ul {
  width: 100%;
  /* height: 0.9rem; */
  box-sizing: border-box;
  /* padding: 0 0.1rem; */
  overflow: hidden;
}
.city_all_div > ul > li {
  list-style: none;
  border-right: 1px solid #e4e4e4;
  border-bottom: 1px solid #e4e4e4;
  float: left;
  box-sizing: border-box;
  width: 0.9375rem;
  text-align: center;
  height: 0.45rem;
  line-height: 0.45rem;
  font-size: 0.14rem;
  color: #666666;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>