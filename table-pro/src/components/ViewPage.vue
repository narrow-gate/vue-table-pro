<template>
    <el-container>
        <el-header>
            <el-row>
<el-col :span="2">
    <slot name="left-field"></slot>
</el-col>
<el-col :span="4">
    <slot name="search-field"></slot>
</el-col>
<el-col :span="12">
    <slot name="filter-field"></slot>
</el-col>
<el-col :span="6">
    <slot name="right-field"></slot>
</el-col>
            </el-row>
        </el-header>
        <el-main>
<slot></slot>
        </el-main>
    </el-container>
</template>
<script>
export default {
    //viewpage对于datatable来说是子组件
}
</script>

