<template>
  <div class="mains">
    <el-button @click="open1" v-show="false">消息</el-button>
    <el-button @click="open2" v-show="false">消息</el-button>
    <div class="head" id="head">
      <div class="content">
         <el-tree
            :data="data2"
            :props="defaultProps"
            node-key="id"
            default-expand-all
            :expand-on-click-node="false"
            :render-content="renderContent">
        </el-tree>
      </div>
    </div>
    <div class="opration" v-show="hide">
        <div class="modify">
          <div class="oprations">
            <div>操作：</div>
            <div @click="fork">X</div>
          </div>
          <div @click="newss">创建子公司</div>
          <div @click="motifss">重命名</div>
          <div @click="deletes">删除公司</div>
        </div>
     </div>
     <div class="opration" v-show="news">
      <div class="modify">
        <div class="oprations">
          <div>{{markName}}</div>
          <div @click="fork">X</div>
        </div>
        <div v-show="addhide">公司：<input type="text" v-model="name"></div>
        <div v-show="addhide">地址：<input type="text" v-model="adress"></div>
        <div v-show="!addhide">名称：<input type="text" v-model="companyName"></div>
        <div>
          <button class="btn" @click="sures">确定</button>
          <button class="btn" @click="cancel">取消</button>
        </div>
      </div>
   </div>
   <div class="addcompany" @click="addhides">
     新增公司
   </div>
  </div>
</template>
<script>
// 获取浏览器可视窗口高度
var liuHeight = document.documentElement.clientHeight - 80;
import htmlToPdf from "../../components/htmlToPdf/htmlToPdf";
// 格式化时间
import util from "../../common/js/util";
import {
  getUserListPage,
  removeUser,
  batchRemoveUser,
  editUser,
  addUser,
  unitTree,
  equipment,
  Alarmrecord,
  saveWarnBatch,
  getWarnData,
  company,
  companysave,
  companydelet
} from "../../api/api";
let id = 1000;

export default {
  data() {
    return {
      message:'提示信息',
      name:'',
      companyName:'',
      addhide:true,
      companys: "",
      ids: "",
      adress: "123",
      markName: "",
      news: false,
      hide: false,
      data2: [],
      defaultProps: {
        children: "children",
        label: "name"
      },

      company: [],
      height: "",
      One_laye: true,
      twolaye: [],
      threelaye: {},
      length: ""
    };
  },
  mounted() {
    this.height = liuHeight + "px";
    this.companyget()
  },
  methods: {
    // 提示框信息
    open1() {
        var message = this.message;
        this.$message({
          message: message,
          type: "warning"
        });
      },
      open2() {
        var message = this.message;
        this.$message({
          message: message,
          type: "success"
        });
      },
    companyget(){
           company().then(data => {
      if (data.code == 1) {
        this.company = data.msg;
        this.data2 = data.msg;
        let twolaye = data.msg[0].children;
        let twolength = twolaye.length;
        console.log(this.company);
        console.log(this.data2);
        for (var i = 0; i < twolength; i++) {
          this.twolaye.push(false);
          let threelaye = data.msg[0].children[i].children;
          let threelength = threelaye.length;
          this.length = threelength;
        }
      } else {
        alert("查无信息");
      }
    });
    },
    append(store, data) {
      // store.append({ id: id++, label: 'testtest', children: [] }, data);
      console.log(data);
      console.log(store);
      this.hide = true;
      this.adress = data.addres;
      this.ids = data.id;
      this.name = data.name;
    },
    newss() {
      console.log(88);
      this.hide = false;
      this.news = true;
      this.markName = "创建子公司";
      this.addhide=true
    },
    motifss() {
      this.hide = false;
      this.news = true;
      this.markName = "重命名";
      this.addhide=true
    },
    addhides() {
      this.hide = false;
      this.news = true;
      this.addhide = false;
      this.markName = "新建公司";
    },
    cancel() {
      
      if(this.markName == "新建公司"){
          this.hide = false;
          this.news = false;
      }else{
       this.hide = true;
       this.news = false;
      }
    },
    sures() {
      console.log(this.adress);
      var that = this;
      let markName = this.markName;
      if (markName == "创建子公司") {
        let para = {
          name: that.name,
          parentId: that.ids,
          addres: that.adress
        };
        companysave(para).then(res => {
          that.companyget()
          that.hide = false;
          that.news = false;
          that.$message({
            type: "success",
            message: "创建子公司成功!"
          });
        });
        return
      } 
      if(markName == "重命名"){
        let para = {
          name: that.name,
          id: that.ids,
          addres: that.adress
        };
        companysave(para).then(res => {
          that.companyget()
          that.hide = false;
          that.news = false;
          that.$message({
            type: "success",
            message: "重命名成功!"
          });
        });
        return
      }else{
        console.log(!that.companyName)
        if(!that.companyName){
          that.message='填写公司名'
          that.open1()
          return
        }
        let para = {
          name: that.companyName,
          parentId: '1'
        };
        companysave(para).then(res => {
          console.log(res)
          that.companyget()
          that.hide = false;
          that.news = false;
          if(res.code==1){
            that.$message({
            type: "success",
            message: "创建成功!"
          });
          }
        });
      }
    },
    deletes() {
      let that=this
      this.$confirm("此操作将永久删除("+that.name+")公司, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          console.log(that.ids)
          let para = {
          id:that.ids
        };
        companydelet(para).then(res => {
          that.companyget()
          this.$message({
            type: "success",
            message: "删除成功!"
          });
          that.hide = false;
          that.news = false;
        });
          
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    remove(store, data) {
      store.remove(data);
    },
    // 叉掉
    fork() {
      this.hide = false;
      this.news = false;
    },
    renderContent(h, { node, data, store }) {
      return (
        <span>
          <span>
            <span>{node.label}</span>
          </span>
          <span style="float: right; margin-right: 20px" class="button">
            <el-button size="mini" on-click={() => this.append(store, data)}>
              操作
            </el-button>
          </span>
        </span>
      );
    }
  }
};
</script>
<style>
#head .button {
  display: none;
}
#head .el-tree-node__content:hover .button {
  display: block;
}
</style>

<style scoped>
button::after {
  border: none;
}
button {
  outline: none;
  border: none;
  list-style: none;
  background-color: #fff;
}
.btn {
  background: #2281de;
  color: #fff;
  margin: 0 10px;
  padding: 6px 20px;
}
.head {
  width: 100%;
  height: 90vh;
  /* border:1px solid red; */
  overflow: scroll;
  padding-bottom: 100px;
}
.mains {
  position: relative;
}
.opration {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.addcompany {
  position: fixed;
  color: #fff;
  background: #2e86df;
  z-index: 9999;
  bottom: 60px;
  right: 160px;
  padding: 10px;
  cursor: pointer;
}
.modify {
  width: 400px;
  /* height: 100px; */
  background: #fff;
  padding: 10px;
}
.modify > div {
  width: 100%;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid #e5e5e5;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.oprations {
  display: flex;
  flex-direction: row;
  justify-content: space-between !important;
  /* justify-content:center; */
  align-items: center;
}
.oprations > div:nth-child(2) {
  display: flex;
  justify-content: center;
  align-items: center;
  padding-left: 40px;
  cursor: pointer;
}
.content {
  width: 100%;
}
</style>
