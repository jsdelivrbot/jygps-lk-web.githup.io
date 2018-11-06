<template>
    <div style="width: 100%;height: 1200px;background: #fff;margin-bottom: 100px; overflow:hidden;" id="s">
        
        <div style="width:100%; height:100%; overflow:hidden;">
          <!-- 卫星图开始 -->
                <div class="content" v-show="markDelets">
                      
                    <div id="allmap">
                      <!-- 显示国外 -->
                      <!-- <div class="button-group" style="z-index:9999">
    <input id="en" type="button" class="button" value="显示英文底图" />
    <input id="zh_en" type="button" class="button" value="显示中英文对照底图" />
    <input id="zh_cn" type="button" class="button" value="显示中文底图" />
  </div> -->
  <!-- 显示国外 -->
                        <div class="delet" @click="mark_Delet" style="z-index:9999;">X</div> 
                    </div>
                    <!-- <div style="width:100%;height:100%;text-align:center;line-height:700px;font-size:13px;color:#1414;" v-show="true">该功能正在研发中...</div> -->
                    <!-- <div class="mark_main" id="mark_main"> -->
                    <!-- </div> -->
                </div>
                <!-- 卫星图结束 -->
            <section class="chart-container" style="background:#F3F6FC;overflow-x:hidden;">
                <!-- 弹出结束 -->
                
                
                <el-row style="margin-bottom: 500px;">
                    <div v-loading="loading" style="pointer-events: none; width: 90%;height:80%;position:fixed;bottom:10%;right:0;z-index: 990;"></div>
                    <div v-for="(item,index) in oprations" :key="index" id="all-box">
                        <el-col :span="8" style="background: #F3F6FC;overflow:hidden;" class="showover">
                            <el-row type="flex" class="row-bg" justify="space-between">
                                <!-- <div style="width:100%;height:300px;position:absolute;right:0;top:0;z-index:99999;" v-loading=loa></div> -->
                                <el-col :span="5">
                                    <div class="grid-content bg-purple chart-head-lt" @click="conmuni()">
                                        <div class="chart-head-title" :title="conmunis[index]">{{conmunis[index]}}</div>
                                        <div class="yuan"></div>
                                    </div>
                                </el-col>
                                <el-col :span="6">
                                    <div class="grid-content bg-purple-light chart-head-cn">报警（{{num[index]}}）</div>
                                </el-col>
                                <el-col :span="6">
                                    <div class="grid-content bg-purple chart-head-rg" @click="markTS(conmunis[index])">
                                        <img src="../../assets/icon/weixing.png" alt="">
                                        <span>卫星图</span>
                                    </div>
                                </el-col>
                            </el-row>
                            <div style="width: 100%;" class="all-box">
                                <el-table :data="newallum[index]" height="307" style="width: 100%;overflow-y:hidden;padding-bottom: 100px;">
                                    <el-table-column label="节点名称" width="120" :show-overflow-tooltip="true">
                                        <template slot-scope="scope">
                                            <span class="blue" v-if="scope.row.levels === 0">{{scope.row.nodename}}</span>
                                            <span class="red" v-else>{{scope.row.nodename}}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="温度(°C)" width="85">
                                        <template slot-scope="scope">
                                            <span class="blue" v-if="scope.row.levels === 0">{{scope.row.tmp1Current}}</span>
                                            <span class="red" v-else>{{scope.row.tmp1Current}}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="湿度(%)" width="90" v-if="isHasHumidity[index]">
                                        <template slot-scope="scope">
                                            <span class="blue" v-if="scope.row.levels === 0">{{scope.row.humidity>100?"":scope.row.humidity}}</span>
                                            <span class="red" v-else>{{scope.row.humidity>100?"":scope.row.humidity}}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="更新时间" width="125" :show-overflow-tooltip="true">
                                        <template slot-scope="scope">
                                            <span class="blue" v-if="scope.row.levels === 0">{{scope.row.tmpUpdateTime}}</span>
                                            <span class="red" v-else>{{scope.row.tmpUpdateTime}}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="趋势线" width="95" fixed="right" style="text-align: center">
                                        <template slot-scope="scope">
                                            <img src="../../assets/tongji.png" alt="" style="width:20px;cursor:pointer;" @click="clickShwo(scope.$index, scope.row)">
                                        </template>
                                    </el-table-column>
                                </el-table>
                            </div>
                        </el-col>
                    </div>
                    <!-- </div> -->
                </el-row>
                
                <!-- 弹出开始 -->
                <div class="content" v-if="delets">
                    <div class="delet" @click="newDelet">X</div>
                    <!-- <div style="width:100%;height:100%;text-align:center;line-height:700px;font-size:13px;color:#1414;" v-show="showData">暂无数据</div> -->
                    <div class="echartS" id="main">
                    </div>
                </div>
                
            </section>
        </div>
    </div>
    
</template>
<!-- <script src="//webapi.amap.com/maps?v=1.3&key=1f0db36b82f9faab094887faee232b52"></script> -->
<script>
// import AMap from 'AMap';   //在页面中引入高德地图
import util from "../../common/js/util";
import axios from "axios";
import echarts from "echarts";
import {
  getUserListPage,
  removeUser,
  batchRemoveUser,
  editUser,
  addUser,
  history,
  allNode,
  unitTree,
  newhistory,
  exportPdf,
  getNode,
  nodeinfo,
  marks
} from "../../api/api";
export default {
  data() {
    return {
      titlemap: "",
      levnum: [],
      newlevnum: [],
      bbb: [[116.405467, 39.907761], [106.405467, 32.907761]],
      conmunis: [],
      conmunisId: [],
      isHasHumidity: [],
      maplan: [],
      inde: 0,
      newallum: [],
      loading: true,
      loa: true,
      aaaa: true,
      oprations: [],
      showData: false,
      value3: "",
      value4: "",
      vehicleId: "",
      tmp1min: "",
      tmp1max: "",
      hum1max: "",
      hum1min: "",
      delets: false,
      markDelets: false,
      newadds: [],
      num: [],
      id: "",
      allnode: [],
      allnum: [],
      chartColumn: null,
      chartBar: null,
      chartLine: null,
      chartPie: null
    };
  },
  created() {
    this.loading = true;
  },
  mounted() {
    this.getUsers();
      
  },
  methods: {
    conmuni() {
      for (var i = 0; i < this.newallum.length; i++) {
        this.conmunis[i] = this.newallum[i][0].unitName;
        // this.conmunisId[i] = this.oprations[i].id;
      }
      console.log(this.newallum.length);
    },
    mark_Delet() {
      this.markDelets = false;
      this.$router.go({name: 'echarts'});
    },
    markTS(index) {
      //   请求
      var unitIds=''
      for(var i=0;i<this.oprations.length;i++){
        if(this.oprations[i].name.replace(/(^\s*)|(\s*$)/g, "")==index){
          unitIds=this.oprations[i].id
        }
      }
      // console.log(index)
      let para = {
        // unitIds: this.oprations[1].id
        unitIds: unitIds
      };
      //查询的结果
      marks(para).then(res => {
        console.log(res);
        // 添加了谷歌地图开始
        var googleLayer = new AMap.TileLayer({
          getTileUrl:
            "http://mt{1,2,3,0}.google.cn/vt/lyrs=s&hl=zh-CN&gl=cn&x=[x]&y=[y]&z=[z]&s=Galile"
        });
        var roadNetLayer = new AMap.TileLayer.RoadNet(); //定义一个路网图层
        //定义谷歌卫星切片图层
        // 添加了谷歌地图结束，下面还有一个
        
        // 卫星图开始
        var map = new AMap.Map("allmap", {
          resizeEnable: true,
          // layers: [new AMap.TileLayer.Satellite()],
          zoom: 11,
          center: [116.397428, 39.90923],
          layers: [googleLayer, roadNetLayer] //设置图层谷歌地图的
        });
        AMap.plugin(["AMap.ToolBar", "AMap.Scale"], function() {
          map.addControl(new AMap.ToolBar());
          map.addControl(new AMap.Scale());
        });
        this.levnum = res.msg.levnum;
        var infoWindow = new AMap.InfoWindow({offset: new AMap.Pixel(0, -30)});
        for (var j = 0; j < this.levnum.length; j++) {
          this.newlevnum = [];
          
          // this.newlevnum.push(this.levnum[j].lon)
          //  this.newlevnum.push(this.levnum[j].lat)
          
          if (!this.levnum[j].lon) {
            this.levnum[j].lon = 116.397428;
          }
          if (!this.levnum[j].lat) {
            this.levnum[j].lat = 39.90923;
          }
          this.newlevnum[0] = this.levnum[j].lon;
          this.newlevnum[1] = this.levnum[j].lat;
          this.titlemap = this.levnum[j].nodename;
          if(this.levnum[j].lon==116.397428&&this.levnum[j].lat==39.90923){
            this.titlemap='该冷库没有经纬度信息'
          }
          console.log(this.newlevnum);
          // this.newlevnum=this.newlevnum
          // 构造点标记
          // var this.bbb=[
          //   [116.405467, 39.907761],
          //   [106.405467, 32.907761]
          // ];

          var marker = new AMap.Marker({
            icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
            position: this.newlevnum,
            title: this.titlemap
          });
          // 标记文本
        //   marker.setLabel({
		    //     offset: new AMap.Pixel(15, 15),
		    //     content: '仓库名：'+this.titlemap
		    // });
          // marker.content = this.titlemap+'num为'+this.levnum[j].num;
          marker.content = '<div id="superman" style="color:#3399FF;width:100%;">库名：'+this.titlemap+'</div><div style="color:green;">正常：'+this.levnum[j].num+'</div><div style="color:red;">报警：'+this.levnum[j].noTnum+'</div>';
          // marker.content = this.titlemap;
          
        marker.on('click', markerClick);
        marker.emit('click', {target: marker});
          map.add([marker]);

          map.setFitView();
    //       // 控制标记文本的样式
    //       setTimeout(()=>{
    //   var supermans=document.getElementsByClassName("amap-marker-label")
    //   supermans[0].style.color='blue'
    //   // supermans[0].style.opacity='0.5';
    //  console.log(supermans[0])
    // },100)
          // js控制地图信息窗口的背景色，要加延时器
    //       setTimeout(()=>{
    //   var supermans=document.getElementsByClassName("amap-info-content")
    //   supermans[0].style.background='red';
    //   supermans[0].style.opacity='0.5';
    //  console.log(supermans[0])
    // },100)
          // js控制地图信息窗口的背景色，要加延时器
        }
        function markerClick(e) {
        infoWindow.setContent(e.target.content);
        infoWindow.open(map, e.target.getPosition());
    }
    
         
    // 显示外国地图要把卫星地图关掉这段打开，上面的div打开
    //  ['en', 'zh_en', 'zh_cn'].forEach(function(btn) {
    //   var button = document.getElementById(btn);
    //   AMap.event.addDomListener(button,'click',clickListener)
    // });

    // function clickListener() {
    //     map.setLang(this.id);
    // }
    // 显示外国地图
    map.setFitView();
        //构建信息窗体中显示的内容
        // 卫星图结束
      });

      this.markDelets = true;

      // let para = {
      //     unitIds: 1
      // };
    },
    clickShwo(index, row) {
      // alert(row)
      this.vehicleId = row.id;
      this.delets = true;
      console.log(row);
      this.drawChart();
    },
    newDelet() {
      this.delets = false;
    },
    getUsers() {
      this.loading2 = true;
      unitTree().then(data => {
        this.loading2 = true;
        if (data.data.code == -1) {
          this.$message({
            message: msg,
            type: "error"
          });
        } else {
          this.$nextTick(() => {
            this.oprations = data.data.msg;
            this.loading = false;
            console.log(this.oprations);
            //  alert(this.oprations[0].id)
            for (var i = 0; i < this.oprations.length; i++) {
              var that = this;
              // that.nodename = that.filters.name;
              let para = {
                page: 1,
                size: 20,
                unitIds: this.oprations[i].id,
                nodename: that.nodename
              };
              nodeinfo(para).then(res => {
                this.loading2 = false;
                // this.allnum = res.msg.rows;
                this.newallum.push(res.msg.rows);
                this.isHasHumidity.push(res.msg.isHasHumidity);
                var alllength = res.msg.rows.length;

                this.inde = 0;
                for (var j = 0; j < alllength; j++) {
                  if (res.msg.rows[j].levels > 0) {
                    this.inde++;
                  }
                  // return this.inde
                }

                this.num.push(this.inde);

                // alert(this.oprations.length)
                // 大bug，只能显示19条数据，admin账号首页头部信息显示不全
                for (var h = 0; h < this.oprations.length; h++) {
                  this.conmunis[h] = this.newallum[h][0].unitName;
                  this.conmunisId[h] = this.oprations[h].id;
                }
                
                  console.log(this.newallum[h][0].id);
                // 大bug，只能显示19条数据，admin账号首页头部信息显示不全
                // this.conmuni()
              });
            }
            //查询的结果
          });
        }
      });
      this.drawChart();
    },
    displayEchart(data) {
      // alert(data)
      this.myChart = echarts.init(document.getElementById("main"));
      var tmp1Array = [];
      var tmp0max = tmp1Array[1];
      var tmp0min = tmp1Array[1];
      var hum0max = tmp1Array[1];
      var hum0min = tmp1Array[1];
      var timeArray = [];
      var hum1Array = [];
      //  console.log("data=",hum2Array);
      var flag = true;
      for (var i = 0; i < data.length; i++) {
        if (data[i].temperature1) {
          tmp1Array[i] = data[i].temperature1;
        } else {
          tmp1Array[i] = "";
        }
        //湿度1
        if (data[i].hum) {
          hum1Array[i] = data[i].hum;
        } else {
          hum1Array[i] = "";
        }
        timeArray[i] = data[i].gpsTime;

        // 最大温度
        if (tmp0max > tmp1Array[i]) {
          tmp0max = tmp0max;
        } else {
          tmp0max = tmp1Array[i];
        }
        // 最小温度
        if (tmp0min < tmp1Array[i]) {
          tmp0min = tmp0min;
        } else {
          tmp0min = tmp1Array[i];
        }
        // 最大湿度
        if (hum0max > hum1Array[i]) {
          hum0max = hum0max;
        } else {
          hum0max = hum1Array[i];
        }
        // 最小湿度
        if (hum0min < hum1Array[i]) {
          hum0min = hum0min;
        } else {
          hum0min = hum1Array[i];
        }
      }
      this.tmp1max = tmp0max;
      this.tmp1min = tmp0min;
      this.hum1max = hum0max;
      this.hum1min = hum0min;
      var hum1 = {
        name: "湿度一",
        type: "line",
        data: hum1Array,
        itemStyle: {
          normal: {
            color: "#F9BF66",
            lineStyle: {
              color: "#F9BF66"
            }
          }
        },
        markPoint: {
          data: [
            {
              type: "max",
              name: "最大值"
            },
            {
              type: "min",
              name: "最小值"
            }
          ]
        }
      };

      var tmp1 = {
        name: "温度一",
        type: "line",
        data: tmp1Array,
        itemStyle: {
          normal: {
            color: "#2F89E2",
            lineStyle: {
              color: "#2F89E2"
            }
          }
        },
        markPoint: {
          data: [
            {
              type: "max",
              name: "最大值"
            },
            {
              type: "min",
              name: "最小值"
            }
          ]
        }
      };
      const option = {
        title: {
          text: "▎趋势图",
          textStyle: {
            fontSize: 14
          }
        },
        tooltip: {
          trigger: "axis"
        },
        legend: {
          data: ["温度一", "湿度一"]
        },
        toolbox: {
          show: true,
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            data: (function() {
              var list = [];
              for (var i = 0; i < data.length; i++) {
                list.push(data[i].gpsTime);
              }
              return list;
            })()
          }
        ],
        yAxis: [
          {
            type: "value",
            name: "（°C）",
            min: this.tmp1min,
            max: this.tmp1max,
            interval: 2,
            axisLabel: {
              formatter: "{value} "
            }
          },
          {
            type: "value",
            name: "（%）",
            min: 0,
            max: 100,
            interval: 20,
            axisLabel: {
              formatter: "{value} "
            }
          }
        ],

        series: [tmp1, hum1]
      };
      this.myChart.setOption(option);
    },
    drawChart() {
      var that = this;
      if (this.value3 == "") {
        this.value3 = new Date(new Date().getTime() - 24 * 60 * 60 * 1000);

        this.value4 = new Date(new Date().getTime());
      }
      that.value3 =
        !that.value3 || that.value3 == ""
          ? ""
          : util.formatDate.format(new Date(that.value3), "yyyy-MM-dd hh:mm");
      that.value4 =
        !that.value4 || that.value4 == ""
          ? ""
          : util.formatDate.format(new Date(that.value4), "yyyy-MM-dd hh:mm");
      let para = {
        size: 99,
        page: 1,
        startTime: this.value3,
        endTime: this.value4,
        vehicleId: this.vehicleId
      };
      //查询的结果
      this.listLoading = true;
      history(para).then(res => {
        console.log(res);
        console.clear()
        this.listLoading = false;
        var data = res.msg.data;
        this.all_node = res.msg.data;
        if (res.msg.total == 0) {
          this.showData = true;
        }
        if (res.code == 1) {
          this.total = res.msg.total;
          if (!this.total) {
            // this.open1();
            // return;
          }
          this.displayEchart(data);
        } else {
          //   this.open1()
        }
      });
    },
    drawCharts() {
      this.displayEchart();
    }
  }
};
</script>
<style>
#all-box .showover{
  height: 360px
}
 
/* #all-box .showover:hover{
  height: 362px;
} */

.all-box .el-table td{
  height:36px;
}
.all-box .el-table__fixed-right-patch{
  height:0px;
}
.amap-info-content{
  background: #F0F0F0;
}
.all-box {
  padding: 0 15px 0 0;
  border: none;
}

.all-box .el-table__body-wrapper {
  height: 240px;
  /* padding-bottom: 20px; */
}

.all-box .el-table th > .cell {
  background-color: #fff;
  line-height: 30px;
}

.all-box .el-table {
  overflow-y: hidden;
}

.el-table th.is-leaf {
  border-bottom: none;
}

.all-box head {
  margin: 0;
}

.all-box .el-table th.is-leaf {
  background: #fff;
  border: none;
}

.all-box .el-table td {
  border: none;
}

.all-box .el-table {
  border: none;
}

.all-box .el-table th > .cell {
  width: 100px;
}

.all-box .gutter {
  width: 0;
  background: #fff;
}

.all-box .el-table {
  overflow-y: hidden;
}

.all-box .el-table__body {
  width: 800px;
}
#s .el-table__fixed {
    position: absolute !important;
    top: 0 !important;
    left: 0 !important;
    box-shadow: none !important;
    -moz-box-shadow: none !important;
    -webkit-box-shadow: none !important;
    -moz-box-shadow: none !important;
    overflow-x: hidden !important;
  }

  #s .el-table__fixed-right {
    position: absolute !important;
    top: 0 !important;
    right: 0 !important;
    box-shadow: none !important;
    -moz-box-shadow: none !important;
    -webkit-box-shadow: none !important;
    -moz-box-shadow: none !important;
    overflow-x: hidden !important;
  }
</style>
<style scoped>
/*  */

.el-row {
  /* margin-bottom: 20px */
}

/* .el-row last-child {
      margin-bottom: 0;
    } */

.el-col {
  /* border-radius: 4px; */
}

.bg-purple-dark {
  /* background: #99a9bf; */
}

.bg-purple {
  /* background: #d3dce6; */
}

.bg-purple-light {
  /* background: #e5e9f2; */
}

.grid-content {
  border-radius: 4px;
  min-height: 28px;
}

.row-bg {
  /* padding: 10px 0; */
  background-color: #fff;
}

/*  */

.chart-container {
  width: 100%;
  float: left;
  position: relative;
  height: 100%;
  overflow-y: scroll;
}

.chart-container .el-col {
  margin: 0;
  padding: 10px 0 0px 10px;
}

.content {
  width: 100%;
  height: 93%;
  box-sizing: border-box;
  /* border: 1px solid black; */
  position: fixed;
  top:49px;
  bottom: 6px;
  left:130px;
  right: -1px;
  z-index: 999;
  background: white;
}

/*.chart div {
        height: 400px;
        float: left;
    }*/

.el-col {
  padding: 30px 0 30px 20px;
}

.lt {
  float: left;
}

.rg {
  float: right;
}

.cn {
  margin: 0 auto;
}

.chart-head {
  width: 100%;
  height: 31px;
  position: relative;
  /* position: absolute; */
  top: 0;
  left: 0;
  background: #fff;
}

.chart-head-rg {
  font-size: 12px;
  /* position: absolute; */
  /* right: 53px;
        top: 19px; */
  color: #019fe7;
  cursor: pointer;
}

.chart-head-cn {
  /* position: absolute; */
  /* left: 200px; */
  color: red;
  /* top: 15px; */
}

.chart-head-title {
  margin-left: 4px;
  width: 90px;
  line-height: 31px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 12px;
}

.chart-head-lt {
  margin-left: -9px;
  color: aliceblue;
  background: #019fe7;
  /* padding: 5px; */
  /* /* position: relative; */
  height: 15px;
  border-radius: 0 20px 20px 0;
}

.red {
  color: red;
}

.blue {
  color: black;
}

.delet {
  width: 60px;
  height: 60px;
  cursor: pointer;
  position: fixed;
  right: 10px;
  text-align: center;
  line-height: 60px;
  border-radius: 50%;
  background: #fff;
  z-index: 9999;
}
.echartS {
  margin: 60px 0 0 60px;
  width: 90%;
  height: 560px;
  /* background: black; */
}
#allmap {
  height: 100%;
  overflow: hidden;
}
</style>