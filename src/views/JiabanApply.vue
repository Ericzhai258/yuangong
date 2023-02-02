<template>
    <el-form ref="apply" :model="apply" label-width="100px">
        <el-form-item label="加班开始时间">
          <el-col :span="10">
          <el-date-picker type="date" placeholder="加班开始日期" v-model="apply.timeBegin" style="width: 100%;"></el-date-picker>
          </el-col>
          <el-col :span="10">
          <el-date-picker type="date" placeholder="加班结束日期" v-model="apply.timeEnd" style="width: 100%;"></el-date-picker>
          </el-col>
        </el-form-item>
        
        
        
        <el-form-item>
            <el-button type="primary" @click="onSubmit">立即创建</el-button>
            <el-button @click="resetForm">重置</el-button>
        </el-form-item>
        </el-form>
    </template>
    <script>
      export default {
        data() {
          return {
            apply: {}
          }
        },
        created() {
          if(this.$route.query.type!=null){
            this.apply.type = this.$route.query.type;
          }
        },
        
        methods: {
          onSubmit() {
            
            var empId = window.sessionStorage.getItem('empId');
            this.apply.startTime = this.apply.timeBegin.toISOString().substring(0,10) +" "+ this.apply.timeEnd.toISOString().substring(11,19);
            console.log('applyJiaban/'+empId+'/'+this.apply.timeBegin+'/'+this.apply.timeEnd);
            this.$http.post('applyJiaban/'+empId+'/'+this.apply.timeBegin+'/'+this.apply.timeEnd).then(res => {
              if (res.data) {
                this.$message({
                  message: '添加成功',
                  type: 'success'
                });
                this.$router.push('/apply/applyList');
              } else {
                this.$message({
                  message: '添加失败',
                  type: 'error'
                });
              }
            });
            // this.$http.get('queryAllEmployeeInfo').then(res => {
            //   console.log(res);
            // });
          },
          resetForm() {
            this.apply={};
          }
        }
      }
    </script>