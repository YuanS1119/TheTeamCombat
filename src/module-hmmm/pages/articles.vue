<template>
  <div class="dashboard-container">
    <el-row>
      <el-button type="info" plain>新增面试技巧</el-button>
    </el-row>
    <el-row class="search">
      <div class="left">
        <label>关键字</label>
        <el-input class="input" ref="iptValue" v-model="page.keyword" placeholder="请输入题目编号/题干"></el-input>
      </div>
      <div class="right">
        <el-button @click="delData">清除</el-button>
        <el-button type="primary" @click="getTableData">搜索</el-button>
      </div>
    </el-row>
    <el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="id" label="序号" width="95"></el-table-column>
        <el-table-column prop="title" label="标题" width="550"></el-table-column>
        <el-table-column prop="visits" label="阅读数" width="100"></el-table-column>
        <el-table-column prop="state" label="状态" :formatter="formatter" width="95"></el-table-column>
        <el-table-column prop="creator" label="录入人" width="100"></el-table-column>
        <el-table-column label="操作" width="200">
          <template>
            <el-button type="text" size="small">预览</el-button>
            <el-button type="text" size="small">删除</el-button>
            <el-button type="text" size="small">修改</el-button>
            <el-button type="text" size="small">禁用</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
    <el-row style="float:right;margin-top: 15px;">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="changePage"
        :current-page="page.page"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="page.pagesize"
        layout="sizes, prev, pager, next, jumper"
        :total="page.counts"
      ></el-pagination>
    </el-row>
  </div>
</template>

<script>
import { list, simple } from '@/api/hmmm/articles'
export default {
  name: 'ArticlesList',
  data() {
    return {
      tableData: [],
      page: {
        counts: 1, // 总条数
        pagesize: 5, // 页大小
        page: 1, // 当前页
        keyword: '',
        pages: 1
      },

      searchList: []
    }
  },
  methods: {
    delData() {
      this.$refs.iptValue.clear()
      this.getTableData()
    },
    async getTableData() {
      const res = await list(this.page)
      this.tableData = res.data.items
      this.page.counts = res.data.counts
    },
    // table内部过滤器
    formatter(row, column, cellValue, index) {
      return cellValue ? '禁用' : '启用'
    },
    // 分页
     async handleSizeChange (val) {
        this.page.pagesize = val
        this.getTableData()
    },
    async changePage(newpage) {
      this.page.page = newpage
      this.getTableData()
    } 
  },
  // computed: {
  //   search() {
      
  //   }
  // },
  async created() {
    this.getTableData()
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
        width: 48px;
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
