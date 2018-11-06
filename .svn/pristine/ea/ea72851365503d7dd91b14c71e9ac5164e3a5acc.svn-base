<template>
        <div class="password-box">
          <el-button @click="open1" v-show="false">消息</el-button>
          <el-button @click="open2" v-show="false">消息</el-button>
          <el-button @click="open3" v-show="false">消息</el-button>
          <el-button @click="open4" v-show="false">消息</el-button>
          <el-button @click="open5" v-show="false">消息</el-button>
          <el-button @click="open6" v-show="false">消息</el-button>
          <el-button @click="open7" v-show="false">消息</el-button>
            <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm">
          <el-form-item label="原始密码" prop="oldpass">
          <el-input type="password" v-model="ruleForm2.oldpass"></el-input>
        </el-form-item>
        <el-form-item label="新密码" prop="pass">
          <el-input type="password" v-model="ruleForm2.pass" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="确认新密码" prop="checkPass">
          <el-input type="password" v-model="ruleForm2.checkPass" auto-complete="off"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
          <el-button @click="resetForm('ruleForm2')">重置</el-button>
        </el-form-item>
      </el-form>
  </div>

</template>
<script>
import { changePassword } from "../api/api";
export default {
  data() {
    var checkOldpass = (rule, value, callback) => {
      setTimeout(() => {
        if (value === "") {
          callback(new Error("请输入原始密码"));
        } else {
          callback();
        }
      }, 1000);
    };
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else {
        if (this.ruleForm2.checkPass !== "") {
          this.$refs.ruleForm2.validateField("checkPass");
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== this.ruleForm2.pass) {
        callback(new Error("新密码两次输入不一致!"));
      } else {
        callback();
      }
    };
    return {
      a:true,
      ruleForm2: {
        pass: "",
        checkPass: "",
        oldpass: ""
      },
      rules2: {
        pass: [{ validator: validatePass, trigger: "blur" }],
        checkPass: [{ validator: validatePass2, trigger: "blur" }],
        oldpass: [{ validator: checkOldpass, trigger: "blur" }]
      }
    };
  },
  methods: {
    open1() {
        this.$message({
          message: "原始密码填写有误",
          type: "warning"
        });
      },
      open2() {
        this.$message({
          message: "密码修改成功",
          type: "success"
        });
      },
      open3() {
        this.$message({
          message: "2次新密码填写不一致",
          type: "warning"
        });
        
      },
      open4() {
        this.$message({
          message: "原始密码不能为空",
          type: "warning"
        });
        
      },
      open5() {
        this.$message({
          message: "新密码不能为空",
          type: "warning"
        });
      },
      open6() {
        this.$message({
          message: "新密码不能为空",
          type: "warning"
        });
      },
      open7() {
        this.$message({
          message: "确认新密码不能为空",
          type: "warning"
        });
      },
    submitForm(formName) {
      // 修改密码
      var passwode = {
        oldPassword: this.ruleForm2.oldpass,
        password: this.ruleForm2.pass,
        password_confirmation: this.ruleForm2.checkPass
      };
      changePassword(passwode).then(data => {
        
        this.logining = false;
        let { code, msg, name } = data;
        if (code == -1) {
          if(this.ruleForm2.pass!=""&&this.ruleForm2.checkPass!=""){
            if(this.ruleForm2.pass===this.ruleForm2.checkPass){
            this.open1()
          }else{
            this.open3()
          }
          }
          if(this.ruleForm2.oldpass===""){
            this.open4()
          }
          if(this.ruleForm2.pass===""){
            this.open6()
          }
          if(this.ruleForm2.checkPass===""){
            this.open7()
          }
          if(this.ruleForm2.pass===""&&this.ruleForm2.checkPass===""){
            this.open5()
          }
          if(this.ruleForm2.pass===""&&this.ruleForm2.checkPass===""&&this.ruleForm2.oldpass===""){
            this.open4()
          }
          
        } else {
          this.a=false;
          this.$emit('ievent',this.a);
         this.open2()
        this.ruleForm2.pass=""
        this.ruleForm2.checkPass=""
        this.ruleForm2.oldpass=""
        //  this.$emit('a-msg',this.a);

          //        location.href='https://www.baidu.com'; 跳外链
        }
      });
      // this.$refs[formName].validate((valid) => {
      //   if (valid) {
      //     alert('密码修改成功！');
      //   } else {
      //     console.log('密码修改失败!');
      //     return false;
      //   }
      // });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>
<style>
.password-box {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.demo-ruleForm .el-input{
  width:100%;
}
</style>