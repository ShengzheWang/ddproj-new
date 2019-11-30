<template>
  <div id="app">
    <div class="top-bar">
      <div class="top-left-bar">
        <span class="bar-title">
          <!-- 叮当口袋管理后台 -->
          后台管理
        </span>
      </div>

      <div class="top-right-bar" style="z-index: 1000">
<!--          <img src="./assets/用户.png" style="width: 30px; height:30px" />-->
<!--          <div style="margin-left: 15px;">-->
<!--            <span style="font-size: 18px;">Admin</span>-->
<!--          </div>-->
         <i class="el-icon-switch-button" style="color: dodgerblue;font-size: 30px;"/>

      </div>
    </div>
    <div class="content-container" style="position:fixed; z-index: 100">
      <div class="left-menu"
           style="height: 100vh"
      >
        <el-menu
                router
                unique-opened
                default-active="2"
                style="height: 100vh"
                class="el-menu-vertical-demo">
          <el-menu-item index="HomePage">
            <i class="el-icon-s-home"></i>
            <span>首页</span>
          </el-menu-item>
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-user-solid"></i>
              <span>用户管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="UserList">用户列表</el-menu-item>
              <el-menu-item index="UserFeedBack">用户反馈</el-menu-item>
              <el-menu-item index="MajorManage">专业管理</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-menu-item index="ShopManage">
            <i class="el-icon-s-goods"></i>
            <span slot="title">商家管理</span>
          </el-menu-item>
          <el-menu-item index="4" >
            <i class="el-icon-document"></i>
            <span slot="title">商品管理</span>
          </el-menu-item>
          <el-submenu index="5">
            <template slot="title">
              <i class="el-icon-star-on"></i>
              <span>活动管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="5-1">校园活动</el-menu-item>
              <el-menu-item index="5-2">砍价活动</el-menu-item>
              <el-menu-item index="5-2">抽奖管理</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="6">
            <template slot="title">
              <i class="el-icon-s-promotion"></i>
              <span>运营管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="RmpManage">轮播图管理</el-menu-item>
              <el-menu-item index="TodayManage">今天吃什么</el-menu-item>
              <el-menu-item index="PassageManage">文章管理</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-menu-item index="OrderManage" >
            <i class="el-icon-s-marketing"></i>
            <span slot="title">交易管理</span>
          </el-menu-item>
          <el-submenu index="8">
            <template slot="title">
              <i class="el-icon-s-tools"></i>
              <span>系统管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="AdminManage">人员管理</el-menu-item>
              <el-menu-item index="RootManage">角色管理</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>

      </div>
      <div class="main-content">
        <router-view />

      </div>


    </div>




  </div>
</template>

<script>


export default {
  name: 'app',
  components: {
  },
  mounted(){
    let accountPayload = {
      account:'test',
      password:'12345678'
    }
    fetch(this.global.base_url+'/admin/login',{
      method:'post',
      mode:'cors',
      body: JSON.stringify(accountPayload),
      headers:{
        'Content-Type':'application/json'
      }
    }).then(res=>res.json()).then((response)=>{
      console.log(response);
      this.global.token ='Bearer ' + response.data.token;
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif, "PingFang SC";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width:100%;
  height:100%;
}

html,body{
  margin:0;
  padding:0;
  width: 100%;
  height:100%

}

.main-content{
  width: 83%;
  height: 100vh;
  overflow-y: scroll;
  padding-bottom: -100px;
}

.top-bar{
  width: 100%;
  height: 60px;
  background-color: white;
  /*background-color: dimgray;*/
  position: fixed;
  top:0;
  box-shadow: 2px 2px 2px whitesmoke;
}

  .top-left-bar{
    width: 200px;
    margin-left: 20px;
    height:60px;
    float: left;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .bar-title{
    font-family: "PingFang SC";
    font-size: 20px;
    color:dimgray;
    /*font-weight: bold;*/
  }


  .top-right-bar{
    width: 200px;
    height: 60px;
    margin-right: 20px;
    float: right;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    /*background-color: #42b983;*/
    flex-direction: row;
  }


  .content-container{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    z-index: 10;
    margin-top: 65px;
  }


  .left-menu{
    width: 17%;
    height: 100%;
    /*overflow-y: scroll;*/
  }
</style>
