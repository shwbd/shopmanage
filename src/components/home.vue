<template>
  
   <el-container class = 'container'>
       <!-- 头部 -->
  <el-header >
      <el-row>
<el-col :span= '4'>
    <img src="../../logo.png" alt="">
</el-col>
<el-col :span = '19' class = 'middle'>
<h2>电商后台管理系统</h2>
</el-col>
<el-col :span = '1'>
<div class = 'grid-content bg-purple'>
<a href="#" @click.prevent = 'handleLoginout()'
class = 'loginout'>退出</a>
</div>
</el-col>
      </el-row>
      <!-- <img src="../../logo.png" alt="">
      <span class = 'middle'>电商后台管理系统</span>
      <a href="" class = 'loginout'>退出</a> -->
  </el-header>
  <el-container>
      <!-- 导航 -->
    <el-aside width="200px" class = 'aside'>
          <el-menu
      default-active="1"
     :router ='true'
     :unique-opened ='true'
     >
      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>用户管理</span>
        </template>
          <el-menu-item index="1-1">用户列表</el-menu-item>
         
      </el-submenu>
      <el-submenu index="2">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>权限管理</span>
        </template>
          <el-menu-item index="1-1">角色列表</el-menu-item>
          <el-menu-item index="1-1">权限列表</el-menu-item>

      </el-submenu>
      <el-submenu index="3">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>商品管理</span>
        </template>
          <el-menu-item index="1-1">商品列表</el-menu-item>
          <el-menu-item index="1-1">分类参数</el-menu-item>
          <el-menu-item index="1-1">商品分类</el-menu-item>

      </el-submenu>
      <el-submenu index="4">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>订单管理</span>
        </template>
          <el-menu-item index="1-1">订单列表</el-menu-item>
      </el-submenu>
       <el-submenu index="5">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>数据统计</span>
        </template>
          <el-menu-item index="1-1">我也不知道</el-menu-item>
      </el-submenu>
    </el-menu>
    </el-aside>
    <el-main class = 'main'>商品</el-main>
  </el-container>
</el-container>

</template>

<script>
export default {
    beforeMount(){
        if (!localStorage.getItem('token')) {
            this.$router.push({
                name:'login'
            })
        }
    },
    methods:{
        handleLoginout () {
        localStorage.clear();
        this.$router.push({
            name:'login'
        });
        this.$message.warning('退出成功')
    }
    }
    
}
</script>

<style>
 .container {
    background-color: #ccc;
    height: 100%;
} 
.middle {
   text-align:center;
   line-height: 60px;
}
.aside{
    /* background-color: blue; */
    
}
.main{
    background-color: red;
   
} 
.loginout {
  line-height: 60px;
  text-decoration: none;
}
</style>
