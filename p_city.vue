// 模板
<template>
  <div id="app">
      <!-- 头部的登录注册 -->
      <div id="top_1">
          <p class="p-left" @click="backcity()">ele.me</p>
          <p class="p-right" v-if="my">登录|注册</p>
          <p class="p-right el-icon-user" v-if="!my"></p>
      </div>
      <!-- 无意义 -->
      <div id="top_2">
          <p class="p-left">当前定位城市：</p>
          <p class="p-right">定位不准时，请在城市列表中选择</p>
      </div>
      <!-- 获取定位城市 跳转到所选城市的搜索页面 -->
      <div id="top_3">
          <!--  根据id展示定位城市，默认是郑州-->
          <p class="p-left" @click="getCityName(citys_ID)">{{citys_GPS.name}}</p>
          <p class="el-icon-arrow-right p-right" @click="getCityName(citys_ID)"></p>
      </div>
      <!-- 展示所有的热门城市 -->
      <ul id="wrap">
          <p class="p-one">热门城市</p>
          <li :key="i" v-for="(v,i) in citys_hot" @click="getCityName(v.id)" class="p-two" v-show="show1">
              {{v.name}}
          </li>
      </ul>
      <!-- 按大写字母排序，展示所有的城市 -->
      <div id="bottom_ul">
          <div :key="i" v-for="(v,i) in citys_ABC" id="bottom">
          <p class="p-one">
              {{v}} <span v-if="v=='A'" class="span-one">(按字母排序)</span>
          </p>
          <ul>
              <li :key="b" v-for="(a,b) in citys_all[v]" class="p-two p-three" @click="getCityName(a.id)" v-show="show2">
                  {{a.name}}
              </li>
          </ul>
      </div>
      </div>
      
  </div>
</template>
// js导出文件
<script>
export default {
    name:"p_city",
    data() {
        return {
            citys_hot:[],//请求的热门城市
            citys_all:[],//请求的所有城市
            citys_ABC:[],//存储请求的所有城市的排序字母
            citys:[],//排序之后的所有城市
            citys_ID:"",//存放id，默认id是32--郑州
            citys_GPS:{},//存放点击定位的城市数据
            my:true,//判断 是显示 登录还是 我的
            show1:false,//判断 是否加载节点-避免重复跳转页面时不显示数据
            show2:false
        }
    },
    created() {
        this.getCitysHot();
        this.getCitysAll();
        this.get();
        //this.getCityName();
    },
    methods: {
        //获取热门城市信息
        getCitysHot(){
            const api= "https://elm.cangdu.org/v1/cities?type=hot";
            this.$http({
                url:api,
                method:"get"
            }).then(res=>{
                this.citys_hot=res.data;
                this.show1=true;
                this.$nextTick(()=>{});
            });
        },
        // 获取所有城市信息
        getCitysAll(){
            const api="https://elm.cangdu.org/v1/cities?type=group";
            this.$http({
                url:api,
                method:"get"
            }).then(res=>{
                this.citys_all=res.data;
                for (const i in this.citys_all) {
                    this.$set(this.citys_ABC,this.citys_ABC.length,i);
                }
                this.citys=this.citys_ABC.sort();
                this.show2=true;
                this.$nextTick(()=>{});
            });
        },

        //
        get(){
            const api="https://elm.cangdu.org/v1/cities/32";
            this.$http({
                url:api,
                method:"get",
                data:{}
            }).then(res=>{
                this.citys_GPS=res.data;
                this.citys_ID=this.citys_GPS.id;
            });
        },
        //跳转定位城市，a是对应的城市id，用来拼接找到对应城市的端口
        getCityName(a){
            const api="https://elm.cangdu.org/v1/cities/"+a;
            this.$http({
                url:api,
                method:"get",
                data:{}
            }).then(res=>{
                this.show=true;
                this.citys_GPS=res.data;
                this.citys_ID=this.citys_GPS.id;
                console.log(this.citys_ID);
                this.citys_ID=res.data.id;
                this.$router.push({
                    name:"p_cityGPS",
                    query:res.data
                });
            });
        },
        //跳转到elm
        backcity(){
            this.$router.push({name:"p_city"});
        }
        //跳转到登录页面
        //跳转到我的页面
    }
}
</script>
//css样式
<style scoped>
    /* 设置 li所有内容里的样式 超出部分设为省略号 */
    ul>li{
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        box-sizing: border-box;
    }
    /* 头部的登录注册部分 */
    #top_1{
        width: 3.75rem;
        height: 0.457rem;
        background-color:#3190e8;
        color: #f5f5f5;
        position: relative;
    }
    #top_1 p:nth-child(1){
        font-size: 0.17rem;
        font-weight: 400
    }
    #top_1 p:nth-child(2){
        font-size: 0.16rem;
    }
    /* 无意义部分 的样式 */
    #top_2{
        width: 3.75rem;
        height: 0.4rem;
        background-color:white;
        color: #666666;
        position: relative;
        border-bottom: 0.005rem solid rgba(0, 0, 0, 0.3);
        box-sizing: border-box;
    }
    #top_2 p:nth-child(1){
        font-size: 0.13rem;
        font-weight: 600;
       }
    #top_2 p:nth-child(2){
        font-size: 0.1rem;
        font-weight: 900;
        color: #9f9f9f;
    }
    /* 设置 定位城市的部分 */
    #top_3{
        width: 3.75rem;
        height: 0.4rem;
        background-color:white;
        color:#3190e8;
        position: relative;
        border-bottom: 0.02rem solid rgba(0, 0, 0, 0.2);
    }
    #top_3 p:nth-child(2){
        color: #666666;
    }
    /*设置热门城市和展示所有城市的部分 的样式*/
    #wrap,#bottom{
        margin-top: 0.15rem;
        overflow: hidden;
        position: relative;
        background-color: #fff;
    }
    /* 设置 从热门城市开始的 一般样式*/
    .p-one{
        box-sizing: border-box;
        border-top: 0.02rem solid rgba(0, 0, 0, 0.2);
        border-bottom: 0.005rem solid rgba(0, 0, 0, 0.3);
        padding-left: 0.1rem;
        width: 3.75rem;
        line-height: 0.3rem;
        font-size: 0.13rem;
        font-weight: 400;
        color: #666666;
    }
    /* 设置 热门城市所有内容的 一般样式【左浮动】（热门城市和字母排序里颜色是不一样的）*/
    .p-two{
        color: #3190e8;
        box-sizing: border-box;
        width:25%;
        float: left;
        text-align: center;
        line-height: 0.44rem;
        font-size: 0.13rem;
        background-color: #fff;
        border-right: 0.01rem solid #e4e4ee;
        border-bottom: 0.01rem solid #e4e4ee;
    }
    .p-three{
        color: #666666;
    }
    /* 设置 距离页面左边间距的 一般样式 */
    .p-left{
        position: absolute;
        left: 0.1rem;
        top: 50%;
        transform: translateY(-50%);
    }
    /* 设置 距离页面右边间距的 一般样式 */
    .p-right{
        position: absolute;
        right: 0.1rem;
        top: 50%;
        transform: translateY(-50%);
    }
    /* 设置字母A后面按字母排序的样式 */
    .span-one{
        font-size: 0.12rem;
        color: #999;
    }
</style>