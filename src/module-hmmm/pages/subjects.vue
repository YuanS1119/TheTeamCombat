<template>
  <div class="dashboard-container">
    <el-row>
      <el-button type="info" plain>新增学科</el-button>
    </el-row>
    <el-row class="search">
      <div class="left">
        <label>学科名称</label>
        <el-input class="input" ref="ipt" v-model="params.subjectName" placeholder="请输入题目编号/题干"></el-input>
      </div>
      <div class="right">
        <el-button @click="clean">清除</el-button>
        <el-button type="primary" @click="getSubject">搜索</el-button>
      </div>
    </el-row>
    <el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="id" label="序号" width="50"></el-table-column>
        <el-table-column prop="subjectName" label="学科名称" width="100"></el-table-column>
        <el-table-column prop="creator" label="创建者" width="90"></el-table-column>
        <el-table-column prop="addDate" label="创建日期" width="197"></el-table-column>
        <el-table-column prop="isFrontDisplay" label="前台是否显示" width="130"></el-table-column>
        <el-table-column prop="twoLevelDirectory" label="二级目录" width="120"></el-table-column>
        <el-table-column prop=" tags" label="标签" width="100"></el-table-column>
        <el-table-column prop="totals" label="题目数量" width="90"></el-table-column>
        <el-table-column label="操作" width="250">
          <template>
            <el-button type="text" size="small">学科分类</el-button>
            <el-button type="text" size="small">学科标签</el-button>
            <el-button type="text" size="small">修改</el-button>
            <el-button type="text" size="small">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
    <el-row style="float:right;margin-top: 15px;">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="changePage"
        :current-page="params.page"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="params.pagesize"
        layout="sizes, prev, pager, next, jumper"
        :total="params.counts"
      ></el-pagination>
    </el-row>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/subjects'
export default {
  name: 'SubjectsList',
  data() {
    return {
      tableData: [],
      params: {
        pages: 1, // 总页数
        page: 1, // 当前页码
        pagesize: 5, // 每页数量
        counts: 1, // 条数
        subjectName: ''
      }
    }
  },
  methods: {
    async getSubject() {
      let res = await list(this.params)
      this.tableData = res.data.items
      // delete res.data.items
      let {counts, page, pages, pagesize} = res.data
      this.params = {
        counts,
        pages,
        page: Number.parseInt(page),
        pagesize: Number.parseInt(pagesize)
      } 
    },
    changePage(newPage) {
      this.params.page = newPage 
      this.getSubject()
    },
    handleSizeChange(val) {
      this.params.pagesize = val
      this.getSubject()
    },
    clean () {
      this.$refs.ipt.clear()
      this.getSubject()
    }
    
  },
  created() {
    this.getSubject()
  }
}
</script>

<style lang='less' scoped>
.dashboard-container {
  height: 100vh;
  padding: 20px;
  background-color: #fff;
  .search {
    margin: 15px 0;
    .left {
      float: left;
      label {
        font-size: 14px;
        width: 96px;
        height: 28px;
        line-height: 28px;
        text-align: center;
        color: #666;
        font-weight: 400;
        background-color: rgba(242, 242, 242, 1);
      }
      .input {
        width: 162px;
        height: 28px;
        border-radius: 4px;
        font-size: 14px;
      }
    }
    .right {
      float: right;
    }
  }
}
</style>
