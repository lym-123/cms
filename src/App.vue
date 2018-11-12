<template>
  <div class="app">
    <div class="header">
      <div class="title"><i class="fa fa-window-restore"></i>看点咨询精选</div>
      <div class="info">
        <img :src="user.userface" alt="">
        <div class="user">
          <el-dropdown @command="handleCommand">
            <span class="el-dropdown-link">
              {{user.nickname}}<i class="el-icon-arrow-down el-icon--right"></i>
            </span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人中心</el-dropdown-item>
              <el-dropdown-item command='logout'>退出</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
      </div>
    </div>
    <div class="content">
      <div class="left_nav">
        <ul>
          <li :class="{current:currentRoute=='/'}">
            <i class="fa fa-home"></i>
            <router-link to='/'>欢迎页面</router-link>
            <i class="fa fa-angle-right"></i>

          </li>
          <li :class="{current:currentRoute=='/category'}">
            <i class="fa fa-bars"></i>
            <router-link to='/category'>栏目管理</router-link>
            <i class="fa fa-angle-right"></i>
          </li>
          <li :class="{current:currentRoute=='/article'}">
           <i class="fa fa-file-text-o"></i>
           <a href="javascript:void(0)" @click='jump("/article")'>文章管理</a>
           <i class="fa fa-angle-right"></i>
          </li>
          <li :class="{current:currentRoute=='/user'}">
             <i class="fa fa-user"></i>
             <router-link to='/user'>用户管理</router-link>
             <i class="fa fa-angle-right"></i>
          </li>
          <li :class="{current:currentRoute=='/exam'}">
             <i class="fa fa-edit"></i>
             <router-link to='/exam'>考试管理</router-link>
             <i class="fa fa-angle-right"></i>  
          </li>
        </ul>
      </div>
      <div class="right_content">
        <div class="wrapper">
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import axios from '@/http/axios'
  export default {
    methods:{
      jump(url){
        this.$router.push(url)
      },
      handleCommand(command){
        if(command == 'logout'){
          axios.get('/logout').then(()=>{
            localStorage.removeItem('user')
          });
        }
       
        
      }
    },
    created(){
      //处理路由默认显示
      this.currentRoute = this.$route.path;

      //处理用户登录
      let user = JSON.parse(localStorage.getItem('user'));
      if(user){
        this.user = user;
      } else {
        window.vm.currentComponent = 'Login';
      }
    },
    watch:{
      '$route':function(to,from){
        this.currentRoute = to.path;
      }
    },
    data(){
      return {
        msg:'App.vue',
        currentRoute:'/',
        user:{}
      }
    }
  }
</script>

<style>
  html {
    font: normal normal 16px '微软雅黑','Microsoft YaHei';
    color: #666;
  }
  body,ul,ol,dl,p,h1,h2,h3 {
    margin: 0;
    padding: 0;
  }
  ul,ol {
    list-style: none;
  }
  a{
    color: #2d2d2d;
    text-decoration: none;
  }
  div{
    box-sizing: border-box;
  }
  .el-dialog__body {
    padding: 0 20px;
  }
  .header {
    position: absolute;
    width: 100%;
    height: 60px;
    top: 0;
    background-image: url(assets/head1_bg.jpg);
    background-repeat: no-repeat;
    background-size: cover;
    /*background-color: teal;*/
    padding: 0 2em;
  }
  .header .title {
    color: #666666;
    line-height: 60px;
    font-size: 24px;
    float: left;
  }
  .header .info {
    line-height: 60px;
    height: 60px;
    text-align: right;
    

  }
  .header .info>img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin-top: 5px;
    margin-right: 1em;
  }
  .header .info>.user {
    float: right;
    cursor: pointer;
  }
  .header .info > .user .el-dropdown {
    color: #666666;
  }
  
  .content {
    position: absolute;
    width: 100%;
    top: 60px;
    bottom: 0;
  }
  .content > .left_nav {
    width: 180px;
    height: 100%;
    float: left;
    background-image: url(assets/nav1_bg.jpg);
    background-repeat: no-repeat;
    background-size: cover;
  }
  .content > .right_content {
    box-sizing: border-box;
    margin-left: 180px;
    height: 100%;
    background-color: #dfe7e9;
    padding: 1em;
  }
  .content > .right_content > .wrapper {
    box-sizing: border-box;
    width: 100%;
    height: 100%;
    border-radius: 5px;
    background-color: #ffffff;
    padding: .5em;
    overflow-y: auto;
  }
  .left_nav ul {

  }
  .left_nav ul li {
    line-height: 38px;
    text-align: center;
    border-bottom: 1px solid #ededed;
    position: relative;
  }
  .left_nav ul li.current {
    background-color: #dfe7e9;
  }
  .left_nav ul li >i:first-child{
    position: absolute;
    left: 2em;
    font-size: 14px;
    top: 12px;
  }
  .left_nav ul li >i:last-child{
    position: absolute;
    right: 2em;
    font-size: 14px;
    top: 12px;
  }

</style>



