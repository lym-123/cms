<template>
  <div class="app">
    <!-- {{multipleSelection}} -->
    <el-button size='mini' style="margin-bottom:.5em" @click='batchDeleteUser'>批量删除</el-button>
    <el-table :data="users" style="width: 100%" :border='true' @selection-change="handleSelectionChange">
      <el-table-column
        type="selection"
        align="center"
        width="55">
      </el-table-column>
      
      <el-table-column
        prop="username"
        align="center"
        label="用户名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="password"
        align="center"
        label="密码">
      </el-table-column>
      <el-table-column
        prop="nickname"
        align="center"
        label="真实姓名">
      </el-table-column>
      <el-table-column
        prop="email"
        align="center"
        label="邮箱地址"
        width="200">
      </el-table-column>
      <el-table-column
        prop="regTime"
        align="center"
        label="注册时间">
      </el-table-column>
      <el-table-column align="center" label="操作" width="55">
        <template slot-scope='{row}'>
         <!--  <el-button type="text" @click="deleteUser(row.id)">删除</el-button> -->
         <i class="fa fa-trash" @click='deleteUser(row.id)'></i>
        </template>
      </el-table-column>
    </el-table>

  </div>
</template>
<script>
  import axios from 'axios'
  export default {
    methods:{
      // 批量删除用户
      batchDeleteUser(){
        let ids = this.multipleSelection.map((item)=>{
          return item.id;
        });
      },
      // ?????
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
        /* 通过id删除用户 */ 
      deleteUser(id){
        this.$confirm('此操作将永久删除该数据, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(()=>{
          axios.get('/manager/user/deleteUserById?id='+id)
          .then(()=>{
            this.$notify.success({
              title: '成功',
              message: '删除成功！'
            });
            this.findAllUsers();
          })
          .catch(()=>{
            this.$notify.error({
              title: '错误',
              message: '删除失败！'
            });
          });
        })
      },
      /* 查询所有用户 */
      findAllUsers(){
        this.loading=true;
        axios.get('/manager/user/findAllUser')
        .then(({data:result})=>{
          console.log(result);
          this.users = result.data;
        })
        .catch(()=>{
          this.$notify.error({
            title: '错误',
            message: '网络异常！'
          });
        })
        .finally(()=>{
          this.loading=false;
        })
      }
    },
    // 
    created(){
      this.findAllUsers();
    },
    // 数据
    data(){
      return {
        users:[],
        multipleSelection:[]
      }
    }
  }
</script>