<template>
  <div class="dashboard-container">
    <el-row>
      <el-button type="info" plain>新增面试技巧</el-button>
    </el-row>
    <el-row class="search">
      <div class="left">
        <label>关键字</label>
        <el-input class="input" placeholder="请输入题目编号/题干"></el-input>
      </div>
      <div class="right">
        <el-button>清除</el-button>
        <el-button type="primary">搜索</el-button>
      </div>
    </el-row>
    <el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop label="序号" width="100"></el-table-column>
        <el-table-column prop label="标题" width="550"></el-table-column>
        <el-table-column prop label="阅读数" width="100"></el-table-column>
        <el-table-column prop label="状态" width="100"></el-table-column>
        <el-table-column prop label="录入人" width="100"></el-table-column>
        <el-table-column label="操作" width="220">
          <template>
            <el-button type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'ArticlesList',
  data() {
    return {
      tableData: {}
    }
  }
}
</script>

<style lang='less' scoped>
.dashboard-container {
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
        width: 152px;
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
