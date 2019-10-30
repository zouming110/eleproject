<template>
  <div>
    <!-- 顶部导航栏 -->
    <div id="city_nav">
      <span class="el-icon-search city_nav_left"></span>
      <span class="city_nav_center" v-text="cityGPS_xname" v-show="show" @click="back_citylist()"></span>
      <span class="city_nav_right">登录|注册</span>
    </div>

    <!-- 轮播部分 -->
    <div id="lunbo">
      <div class="lunbo_lin"></div>
      <div class="lunbo_div1">
        <div v-for="(v,i) in lunbo_V1_8" :key="i">
          <img :src="'https://fuss10.elemecdn.com/'+v.image_url" />
          <p v-text="v.title"></p>
        </div>
        <!-- <div v-for="(v,i) in lunbo_V9_16" :key="i">
                <img :src="'https://fuss10.elemecdn.com/'+v.image_url">
                <p v-text="v.title"></p>
        </div>-->
      </div>
    </div>

    <!-- 附近商家 -->
    <div id="nearbyShops">
      <div class="nearbyShops_title">
        <i class="el-icon-s-shop"></i>
        <span>附近商家</span>
      </div>
      <div class="nearbyShops_div" v-for="(v,i) in nearbyShop" :key="i" @click="h_nearbyShops(v)" v-show="show1">
        <div class="div_left">
          <img :src="'//elm.cangdu.org/img/'+v.image_path" alt />
        </div>
        <div class="div_right">
          <div class="div_right_p1">         
            <span class="s2"><span class="s1">品牌</span>{{v.name}}</span>
            <span class="s3" >
              <span v-for="(v1,i1) in v.supports" :key="i1">{{v1.icon_name}}</span>
            </span>
          </div>
          <div class="div_right_p2">
            <!-- <el-rate
                    v-model="v.rating"
                    disabled
                    show-score
                    text-color="#ff9900"
                    class="xing"
                    score-template="{value}"> -->
            <span class="s1">{{v.rating}}</span>
            <span class="s2">月售{{v.recent_order_num}}单</span>
            <span class="s4">准时达</span>
            <span class="s3">{{v.delivery_mode.text}}</span>
          </div>
          <div class="div_right_p3">
            <span class="s1">￥{{v.float_minimum_order_amount}}起送 / {{v.piecewise_agent_fee.tips}}</span>
            <span class="s4" v-text="v.order_lead_time"></span>
            <span class="s3">/</span>
            <span class="s2" v-text="v.distance"></span>
          </div>
        </div>
      </div>
    </div>

    <!-- 底部跳转 -->
    <div id="footer">
      <div>
        <span class="el-icon-eleme"></span>
        <p>外卖</p>
      </div>
      <div>
        <span class="el-icon-discover"></span>
        <p>搜索</p>
      </div>
      <div>
        <span class="el-icon-tickets"></span>
        <p>订单</p>
      </div>
      <div>
        <span class="el-icon-user"></span>
        <p>我的</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      show: false,
      show1:false,
      cityGPS_xname: "",
      lunbo_V1_8: [],
      lunbo_V9_16: [],
      nearbyShop:"",
      cityGPS_sl1:"",
      cityGPS_sl2:"",
      // value:""
    };
  },
  created() {
    this.cityGPS_xname = this.$route.query.cityGPS_name;
    this.cityGPS_sl1 = this.$route.query.cityGPS_l1;
    this.cityGPS_sl2 = this.$route.query.cityGPS_l2;
    console.log(this.$route.query.cityGPS_name);
    console.log(this.cityGPS_sl1,this.cityGPS_sl2);
    this.show = true;

    // 调用轮播部分的内容
    this.lunbo();
    // 调用附近商家信息
    this.nearbyShops(this.cityGPS_sl1,this.cityGPS_sl2);
  },
  methods: {
    // 回到城市列表页
    back_citylist() {
      this.$router.push({
        name: "citylist",
        query: {}
      });
    },
    // 请求轮播部分的商品内容
    lunbo() {
      const api = "https://elm.cangdu.org/v2/index_entry";
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        console.log(res.data);
        this.lunbo_V = res.data;
        for (let i = 0; i < 8; i++) {
          this.$set(this.lunbo_V1_8, this.lunbo_V1_8.length, this.lunbo_V[i]);
        }
        for (let i = 8; i < 16; i++) {
          this.$set(this.lunbo_V9_16, this.lunbo_V9_16.length, this.lunbo_V[i]);
        }
        console.log(this.lunbo_V1_8);
        console.log(this.lunbo_V9_16);
      });
    },
    // 附近商家的数据请求
    // https://elm.cangdu.org/shopping/restaurants?latitude=31.22967&longitude=121.4762
    nearbyShops(a,b) {
      const api = "https://elm.cangdu.org/shopping/restaurants?latitude="+a+"&longitude="+b;
      this.$http({
        url: api,
        method: "get", // 请求方式
        data: {}
      }).then(res => {
        //请求数据成功之后的回调
        console.log(res.data);

      // for (let i = 0; i < res.data.length; i++) {
      //      this.value[i] = res.data[i].rating;
      //   }

        this.nearbyShop = res.data;
        console.log(this.nearbyShop);
        this.show1 = true;
      });
    },
    // 跳转至附近商家页面
    h_nearbyShops(a){
        this.$router.push({
        name: "h_nearbyShops",
        query: a
      });
    },
  }
};
</script>

<style scoped>
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
  text-align: center;
}
#city_nav > .city_nav_left {
  float: left;
  color: #ffffff;
  font-size: 0.18rem;
  font-weight: 800;
}
#city_nav > .city_nav_center {
  color: #ffffff;
  font-size: 0.19rem;
  /* font-weight: 800; */
  padding-left: 0.15rem;
}
#city_nav > .city_nav_right {
  float: right;
  color: #ffffff;
  font-size: 0.13rem;
}

/* 轮播部分内容 */
#lunbo {
  width: 100%;
  height: 2.45rem;
  background-color: #fff;
  border-bottom: 1px solid #e4e4e4;
}
.lunbo_lin {
  width: 100%;
  height: 0.45rem;
}
.lunbo_div1 {
  font-size: 0;
  width: 100%;
  height: 1.8rem;
}
.lunbo_div1 > div {
  display: inline-block;
  width: 25%;
  height: 0.8rem;
  margin-top: 0.1rem;
  box-sizing: border-box;
  text-align: center;
}
.lunbo_div1 > div > img {
  width: 45%;
}
.lunbo_div1 > div > p {
  font-size: 0.1rem;
  margin-top: 0.1rem;
  color: #666666;
}

/* 附近商家 */
#nearbyShops {
  width: 100%;
  /* height: 2.45rem; */
  overflow: hidden;
  background-color: #fff;
  border-top: 1px solid #e4e4e4;
  border-bottom: 1px solid #e4e4e4;
  margin-top: 0.1rem;
}
.nearbyShops_title {
  width: 100%;
  height: 0.35rem;
  /* background-color: #e3e3e3; */
  /* line-height: 0.35rem; */
  padding: 0.1rem;
  box-sizing: border-box;
}
.nearbyShops_title i {
  font-size: 0.16rem;
  color: #999999;
}
.nearbyShops_title span {
  font-size: 0.14rem;
  color: #999999;
}
.nearbyShops_div {
  width: 100%;
  padding: 0.15rem 0.1rem;
  box-sizing: border-box;
  overflow: hidden;
}
.div_left {
  float: left;
  width: 20%;
  box-sizing: border-box;
}
.div_left img {
  width: 0.63rem;
  height: 0.63rem;
}
.div_right {
  float: right;
  width: 80%;
  box-sizing: border-box;
}
.div_right_p1 {
  width: 100%;
  height: 0.14rem;
  font-size: 0;
}
.div_right_p1 >.s2>.s1 {
  width: 0.14rem;
  font-size: 0.12rem;
  padding: 0 0.02rem;
  box-sizing: border-box;
  border-radius: 0.02rem;
  background-color: #ffd930;
  color: #333;
  margin-right: 0.05rem;
}
.div_right_p1 > .s2 {
  color: #333;
  font-weight: 800;
  font-size: 0.15rem;
  width: 2rem;
  float: left;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.div_right_p1 > .s3 {
  float: right;
}
.div_right_p1 > .s3 > span {
  font-size: 0.05rem;
  color: #999999;
  border: 1px solid #e4e4e4;
  padding: 0 0.005rem;
  width: 0.05rem;
  height: 0.1rem;
  margin-left: 0.05rem;
}

.div_right_p2 {
  width: 100%;
  height: 0.14rem;
  /* background-color: #e4e4e4; */
  margin-top: 0.15rem;
  font-size: 0;
}
.div_right_p2 > .s1 {
  font-size: 0.12rem;
  color: #ff6000;
}
.div_right_p2 > .s2 {
  font-size: 0.1rem;
  color: #666666;
  margin-left: 0.05rem;
}
.div_right_p2 > .s3 {
  font-size: 0.05rem;
  color: #ffffff;
  float: right;
  height: 0.12rem;
  background-color: #3190e8;
  border-radius: 0.02rem;
  padding: 0.01rem 0.02rem;
  vertical-align: top;
}
.div_right_p2 > .s4 {
  font-size: 0.05rem;
  color: #3190e8;
  float: right;
  height: 0.1rem;
  margin-left: 0.02rem;
  border: 1px solid #3190e8;
  border-radius: 0.02rem;
  padding: 0.01rem 0.02rem;
  vertical-align: top;
}

.div_right_p3 {
  width: 100%;
  height: 0.14rem;
  margin-top: 0.15rem;
  font-size: 0;
}
.div_right_p3 > span {
  font-size: 0.14rem;
}
.div_right_p3 > .s1 {
  font-size: 0.05rem;
  color: #666666;
}
.div_right_p3 > .s2 {
  font-size: 0.05rem;
  color: #999999;
  float: right;
}
.div_right_p3 > .s3 {
  font-size: 0.05rem;
  color: #999999;
  float: right;
  margin: 0 0.03rem;
}
.div_right_p3 > .s4 {
  font-size: 0.05rem;
  color: #3190e8;
  float: right;
}

/* 底部跳转 */
#footer {
  width: 3.75rem;
  height: 0.45rem;
  box-sizing: border-box;
  background-color: #ffffff;
  position: fixed;
  left: 0;
  bottom: 0;
  text-align: center;
  font-size: 0;
}
#footer > div {
  /* background-color: #e2e2e2; */
  box-sizing: border-box;
  display: inline-block;
  width: 25%;
  font-size: 0.12rem;
  height: 100%;
  padding: 0.06rem 0;
  color: #666666;
}
#footer > div > span {
  font-size: 0.22rem;
  /* color: #666666; */
}
#footer > div:first-child > span {
  color: #3190e8;
}
</style>