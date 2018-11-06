<template>
    <div class="block">
    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[15, 20, 25, 30]" :page-size="pageSize" layout=" sizes, prev, pager, next, jumper" :total="totalNum"></el-pagination>
  </div>
</template>
<script>
export default{
    props: {
        totalNum: Number,
  },
    data(){
        return{
            currentPage: 1,//默认显示第一页
            pageSize:15,//默认每页显示10条
        }
    },
    methods: {
      handleSizeChange(val) {//pageSize 改变时会触发

      this.pageSize = val;//每一页显示多少条
        this.$emit('child-say2',val);
      },
      handleCurrentChange(val) {//page 改变时会触发  第几页
        this.page = val;//第几页
        this.$emit('child-say',val);
      }
    },
   mounted(){

  }
}
</script>