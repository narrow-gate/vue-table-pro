<template>
 <el-container>
  <el-header class="head" height="100px">
      <h2>MyProject</h2>
      <h4>this is a tableProject</h4>
    </el-header>
    <el-main>
      <data-table></data-table>
    </el-main>
 </el-container>
</template>

<script>

import DataTable from './components/DataTable'
export default {
  name: 'App',
  components:{
    DataTable
  }
}
</script>

<style>
.head{
  background-color:lightcoral;
  color: white;
  text-align: center;
}
</style>
