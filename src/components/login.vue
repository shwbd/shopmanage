<template>
<div class = 'login-wrap'>
    <el-form
    class = 'login-form'
    label-position="top" label-width="80px" :model="formdata">
        <h2>用户登录</h2>
  <el-form-item label="用户名">
    <el-input v-model="formdata.username"></el-input>
  </el-form-item>
  <el-form-item label="密码">
    <el-input v-model="formdata.password"></el-input>
  </el-form-item>
   <el-button
   @click.prevent = 'handlelogin()'
   type="primary" class = 'login-btn'>登录</el-button>
</el-form>
</div>

</template>

<script>
export default {
  data () {
    return {
      formdata: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    async handlelogin () {
      const res = await this.$http.post(`login`, this.formdata)
      // .then(res => {
      console.log(res)

      const {
        data: {
          data: {token},
          meta: {msg, status}
        }
      } = res
      localStorage.setItem('token', token)
      if (status === 200) {
        this.$router.push({
          name: 'home'
        })
        console.log('成功')
      } else {
        this.$message.error(msg)
      }
      // })
      // .catch(err => {
      //   console.log(err)
      // })
    }
  }
}
</script>

<style>
    .login-wrap {
  height: 100%;
  background-color: #324152;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-form {
  background-color: #ffffff;
  width: 400px;
  border-radius: 5px;
  padding: 30px;
}
.login-btn {
  width: 100%;
}
</style>
