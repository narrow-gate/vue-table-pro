<template>
  <view-page>
    <template slot="left-field">
      <el-button type="danger" icon="el-icon-circle-plus-outline">添加</el-button>
    </template>
  <template slot="search-field">
    <el-input v-model="searchStr" suffix-icon="el-icon-search" placeholder="请输入搜索内容"></el-input>
</template>
    <template slot="filter-field">
      <el-select v-model="filterType" placeholder="选择类型">
        <el-option label="全部" value></el-option>
        <el-option v-for="(status,index) in statuses" :key="status" :value="index" :label="status"></el-option>
      </el-select>
      <el-date-picker type="daterange" start-placeholder="起始时间" end-placeholder="结束时间"></el-date-picker>
    </template>
    <template slot="right-field">
      <el-button type="primary" icon="el-icon-refresh">刷新</el-button>
      <el-button type="warning" icon="el-icon-upload2">导入</el-button>
      <el-button type="success" icon="el-icon-download">导出</el-button>
    </template>
    <el-table :data="sortedData" @sort-change="sortChange">
      <el-table-column type="expand">
        <template slot-scope="scope">
          <!--这里是table组件某一行的row进行传递-->
          <el-card header="书籍内容简介">{{scope.row.content}}</el-card>
        </template>
      </el-table-column>
      <el-table-column label="学习书籍" prop="name"></el-table-column>
      <el-table-column label="作者">
        <template slot-scope="scope">{{scope.row.author.join(',')}}</template>
      </el-table-column>
      <el-table-column label="学习计划状态">
        <template slot-scope="scope">
          <el-tag :type="statusColors[scope.row.status]">{{statuses[scope.row.status]}}</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="学习完成时间">
        <template slot-scope="scope">{{new Date(scope.row.completeDate).toLocaleDateString()}}</template>
      </el-table-column>
      <el-table-column label="操作">
        <template>
          <el-button size="small" type="warning" icon="el-icon-edit"></el-button>
          <el-button size="small" type="danger" icon="el-icon-delete"></el-button>
        </template>
      </el-table-column>
    </el-table>
  </view-page>
</template>

<script>
import ViewPage from "./ViewPage";
export default {
  components: {
    ViewPage
  },
  data() {
    return {
      data: [],
      filterType: "",
       searchStr: "",
      statuses: ["未开始", "进行中", "搁置", "完成"],
      statusColors: ["info", "primary", "warning", "success"],
      sortProp:'',
      sortOrder:''
    };
  },
  mounted() {
    this.update();
  },
  methods: {
    update() {
      this.$ajax
        .get("todos")
        .then(res => {
          if (res.data) {
            this.data = res.data;
          }
        })
        .catch(err => {
          this.$notify({
            type: "error",
            message: err
          });
        });
    },
    sortChange(column){
        this.sortProp=column.sortProp
        this.sortOrder=column.order
    }
  },
  computed: {
    filtedData() {
 return this.data.filter(item=>{
var reg=new RegExp(this.searchStr,'i')
return !this.searchStr||reg.test(item.name)||reg.test(item.author.join(''))
 }).filter(item=>{
     return this.filterType===''||item.status===this.filterType
 }).filter(item=>{
         return !this.filterDates || (this.filterDates[0] <= new Date(item.completeDate) && this.filterDates[1] >= new Date(item.completeDate))
 })
    }
  }
};
</script>

<style>
</style>
