<template>
  <div>
    <el-card class="box">
      <!-- 面包屑 -->
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
        <el-breadcrumb-item>用户列表</el-breadcrumb-item>
      </el-breadcrumb>
      <!-- 搜索 -->
      <el-row class="searchBox">
        <el-col>
          <el-input
            @clear="getAllUsers()"
            clearable
            placeholder="请输入内容"
            v-model="query"
            class="searchInput"
          >
            <el-button @click="searchUser()" slot="append" icon="el-icon-search"></el-button>
          </el-input>
          <el-button 
          @click ='showDiaAddUse()' type="primary">添加用户</el-button>
          <el-button @click="showDiaAddUse()" type="primary"> 用户</el-button>
        </el-col>
      </el-row>
      <!-- 表格 -->
      <el-table :data="list" style="width: 100%">
        <el-table-column prop="id" label="#" width="80"></el-table-column>
        <el-table-column prop="username" label="姓名" width="100"></el-table-column>
        <el-table-column prop="email" label="邮箱" width="140"></el-table-column>
        <el-table-column prop="mobile" label="电话" width="140"></el-table-column>

        <el-table-column label="创建日期" width="200">
          <template slot-scope="scope">{{scope.row.create_time | fmtdate}}</template>
        </el-table-column>
        <el-table-column label="用户状态" width="120">
          <template slot-scope="scope">
              <el-switch@change='changeState(scope.row)'
               v-model="scope.row.mg_state"
                active-color="#13ce66"
                inactive-color="#ff4949"
              >
              </el-switch>
          </template>
        </el-table-column>

        <el-table-column prop="address" label="操作" width="200">
          <template slot-scope="scope">
            <el-button
            @click ='showDiaEditUse' 
            type="primary" icon="el-icon-edit" circle size="mini" plain></el-button>
            <el-button type="success" icon="el-icon-check" circle size="mini" plain></el-button>
            <el-button @click ='showMsgBox(scope.row)' type="danger" icon="el-icon-delete" circle size="mini" plain></el-button>
          </template>
        </el-table-column>

        <el-table-column label="操作" width="200">
          <template slot-scope="scope">
            <el-button 
            @click ='showDiaEdUser(scope.row)' type="primary" icon="el-icon-edit" circle size="mini" plain></el-button>
            <el-button 
            @click="showDiaSetRole(scope.row)" type="success" icon="el-icon-check" circle size="mini" plain></el-button>
            <el-button
              @click="showMsgBox(scope.row)"
              type="danger"
              icon="el-icon-delete"
              circle
              size="mini"
              plain
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页功能 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pagenum"
        :page-sizes="[2, 4, 6, 8]"
        :page-size="2"
        layout="total, sizes, prev, pager, next, jumper"
        :total="10"
      ></el-pagination>
    </el-card>
    <!-- 添加用户 -->
    <el-dialog title="收货地址" :visible.sync="dialogFormVisibleAdd">
      <el-form label-position="left" label-width="80px" :model="formdata">
        <el-form-item label="姓名">
          <el-input v-model="formdata.username"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="formdata.password"></el-input>
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="formdata.email"></el-input>
        </el-form-item>
        <el-form-item label="电话">
          <el-input v-model="formdata.mobile"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleAdd = false">取 消</el-button>
        <el-button type="primary" @click="addUser()">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 对话框 编辑-->
    <el-dialog title="收货地址" :visible.sync="dialogFormVisibleEdit">
      <el-form label-position="left" label-width="80px" :model="formdata">
        <el-form-item label="姓名">
          <el-input disabled v-model="formdata.username"></el-input>
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="formdata.email"></el-input>
        </el-form-item>
        <el-form-item label="电脑">
          <el-input v-model="formdata.mobile"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleEdit = false">取 消</el-button>
        <el-button type="primary" @click="editUser()">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 对话框 -->
    <el-dialog title="分配角色" :visible.sync="dialogFormVisibleRole">
      <el-form :model="formdata" label-position="left" label-width="80px">
        <el-form-item label="用户名">
          {{formdata.username}}
        </el-form-item>
      <el-form-item label="角色" >
        {{selectVal}}
        <el-select v-model="selectVal" placeholder="请选择角色名称">
          <el-option label="请选择" :value="-1"></el-option>
          <!-- <el-option :label="item.roleName" v-for='(item,i) in roles' :key ='item.id'></el-option> -->
          <el-option :label="item.roleName" :value="item.id " v-for="(item,i) in roles" :key='item.id'></el-option>
      </el-select>
    </el-form-item>
  </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleRole">取 消</el-button>
        <el-button type="primary" @click="setRole()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
 export default{
   data(){
     return {
       query: "",
      pagenum: 1,
      pagesize: 2,
      list: [],
      dialogFormVisibleAdd:false,
      formdata:{
          username:'',	
          password:'',	
          email:'',	
          mobile:''
      },
      total: -1,
      list: [],
      dialogFormVisibleAdd: false,
      dialogFormVisibleEdit:false,
      dialogFormVisibleRole:false,
      formdata: {
        username: "",
        password: "",
        email: "",
        mobile: ""
      },
      selectVal:-1,
      roles:[],
      currUserId:-1
     }
   },
  methods:{
   async setRole(){

     const res = await this.$http.put(`users/${this.currUserId}/role`,{
       rid:this.selectVal
     });
     console.log(res);
     const {meta:{msg,status}}=res.data;
     if (status===200) {
       this.dialogFormVisibleRole=false;
       this.$message.success(msg);
     }
     
    },
    // 分配角色
   async showDiaSetRole(user){
     this.currUserId = user.id;
     this.formdata = user;
      this.dialogFormVisibleRole = true;
      const res =await this.$http.get(`roles`)
      // console.log(res);
      const {meta:{msg,status},data}=res.data
      if (status===200){
        this.roles = data;
        console.log(this.roles);
        
      }
      // const res2 = await this.$http.get('users/${user.id}')
      // console.log(res2);
      // const {meta:{msg2,status2},data2} = res2.data;
      // if (status2===200) {
        this.selectVal = res.data.data.rid
      // }
      
      
      
    },
   async changeState(user){
    //  console.log(user);
     
     const res = await this.$http.put(`users/${user.id}/state/${user.mg_state}`)
    //  console.log(res);
     
    },
    async editUser(){
        const res =await this.$http.put(`users/${this.formdata.id}`);
      console.log(res);
      const {meta:{msg,status}} = res.data;
      if (status===200) {
        this.dialogFormVisibleEdit = false; 
        this.getTableData()
      }
      
    },
   showDiaEdUser(user){
     
// create_time: (...)
// email: (...)
// id: (...)
// mg_state: (...)
// mobile: (...)
// role_name: (...)
// username:
     this.formdata = user; 
    //  this.formdata.username = user.username;
    //  this.formdata.password = user.password;
    //  this.formdata.username = user.username;

  this.dialogFormVisibleEdit = true;
    },
    showMsgBox(user) {
      console.log(user);
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(async () => {
          const res = await this.$http.delete(`users/${user.id}`,this.formdata);

          const {
            meta:{ msg, status }
          } = res.data;
          if (status === 200) {
            this.$message.success(msg);
            this.getTableData();
          }
        })
        .catch(() => {
          //
          this.$message.info("取消删除");
        });
    },
    async addUser() {
      // this.formdata.username = {};
      const res = await this.$http.post(`users`, this.formdata);
      console.log(res);
      this.dialogFormVisibleAdd = false;
      this.getTableData();
    },
    showDiaAddUse() {
      this.dialogFormVisibleAdd = true;
    },
    getAllUsers() {
      this.getTableData();
    },
    searchUser() {
      this.pagenum = "1";
      this.getTableData();
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.pagenum = 1;
      this.pagesize = val;
      this.getTableData();
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.pagenum = val;
      this.getTableData();
    },
    async getTableData() {

      const AUTH_TOKEN = localStorage.getItem("token");
      console.log(AUTH_TOKEN);

      this.$http.defaults.headers.common["Authorization"] = AUTH_TOKEN;
      const res = await this.$http.get(
        `users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${
          this.pagesize
        }`
      );
      console.log(res);

      const {
        data,
        meta: { status, msg }
      } = res.data;
      if (status === 200) {
        this.list = data.users;
        this.total = data.total;
        console.log(this.list);
      }
    }
  }
 }
</script>

<style>
.searchBox {
  margin-top: 20px;
}
.searchInput {
  width: 400px;
}
</style>

  


