<template>
<!-- 按钮 -->
    <div>
        <div>
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small" >批量删除</el-button>
        </div>
<!-- /按钮 -->
<!-- 表格 -->
        <el-table :data="category">
            <el-table-column width=30><el-checkbox></el-checkbox></el-table-column>
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="名称"></el-table-column>
            <el-table-column prop="mun" label="数量"></el-table-column>
            <el-table-column prop="icon" label="网址"></el-table-column>
            <el-table-column prop="parentId" label="亲人号"></el-table-column>
            <el-table-column  label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row)">描述</a>
            </template>
            </el-table-column>
        </el-table>
<!-- /表格 -->
<!-- 分页开始 -->
    <el-pagination layout="prev, pager, next" :total="50"></el-pagination>
    <!-- /分页结束 -->
<!-- 模态框 -->
   <el-dialog 
        :title="title" 
        :visible.sync="visible" 
        width ="60%">
        测试:{{form}}
            <el-form  :model="form" label-width="80px">
              <el-form-item label="编号">
                  <el-input v-model="form.id"/>
              </el-form-item>
              <el-form-item label="名称">
                  <el-input v-model="form.name" />
              </el-form-item>
              <el-form-item label="数量">
                  <el-input v-model="form.num"/> 
              </el-form-item>
              <el-form-item label="网址">
                  <el-input v-model="form.icon"/> 
              </el-form-item>
              <el-form-item label="亲人号">
                  <el-input v-model="form.parentId"/> 
              </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button size="small" @click="closeModalHandler">
                    取消
                </el-button>
                <el-button size="small" type="primary" @click="sumbitHandler">
                    确定
                </el-button>    
            </span>

        </el-dialog>
    <!-- /模态框 -->
    </div>
    
</template>
<script>
import request from '@/utils/request' 
import querystring from 'querystring' 
export default {
    data(){
        return{
            title:"录入栏目信息",
            visible:false,
            category:[],
            form:{
                type:"category"
            }
        }
    },
    created(){
        this.loadData();

    },
    methods:{
        sumbitHandler(){
      let url = "http://localhost:6677/category/saveOrUpdate"
      request({
        url,
        method:"post",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data: querystring.stringify(this.form)
      }).then((response)=>{
                //请求结束 模态框关闭
                this.closeModalHandler();
                //提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
    },
    loadData(){
      let url = "http://localhost:6677//category/findAll"
      request.get(url).then((response)=>{
        //箭头函数中的this指向外部函数中的this
        this.category = response.data;

      })
    },
    toAddHandler(){
      this.title="添加栏目信息",
      this.visible = true;

    },
    closeModalHandler(){
      this.visible = false;
    },
    toDeleteHandler(id){
      this.$confirm('此操作将永久删除该文件，是否继续？','提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                this.$message({
                    type: 'success',
                    message: '删除成功！'
                });
            })
    },
    toUpdateHandler(row){
      this.title="修改栏目信息",
      this.visible=true;

    }
  },


    
}
</script>
<style scoped>

</style>
