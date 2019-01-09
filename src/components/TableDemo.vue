<!--suppress ALL -->
<template>
  <!--table模板-->
  <table-layout @sizeChange="handleSizeChange" @currentChange="handleCurrentChange" @handleRowClick="handleRowClick" @handleCellClick="handleCellClick" :spanMethod="spanMethod"  :pageObj="pageObj" :headers="headers" @selectionChange="handleSelectionChange" :tableData="tableData">
    <template slot="top-left">
      左边插槽
    </template>
    <template slot="top-right">
      右边插槽
    </template>
    <!---->
    <template slot="expand" slot-scope="{scope}">
      <el-form label-position="left" inline class="demo-table-expand">
        <el-form-item label="名称">
          {{scope.row.name}}
        </el-form-item>
      </el-form>
    </template>
    <!--姓名电话-->
    <template slot="name-phone" slot-scope="{scope}">
      <div> <i class="el-icon-time"></i>{{scope.row.name}}</div>
      <div> <i class="el-icon-time"></i>{{scope.row.phone}}</div>
    </template>
    <!--头像-->
    <template slot="avatar" slot-scope="{scope}">
      <img :src="scope.row.avatar" alt="头像">
    </template>
    <!--操作-->
    <template slot="operator" slot-scope="{scope}">
      <el-button @click="handleView(scope.row)">查看</el-button>
      <el-button @click="handleDel(scope.row)">删除</el-button>
    </template>
  </table-layout>
</template>

<script>
import { TABLE_MIXINS } from './../mixins/table_mixins'
import TableLayout from './table/TableLayout'
export default {
  name: 'table-demo',
  components: { TableLayout },
  mixins: [ TABLE_MIXINS ],
  data () {
    return {
      headers: [
        { type: 'selection' },
        { type: 'expand', slot: 'expand' },
        { type: 'index', fixed: true },
        { prop: 'id', label: 'id' },
        { prop: 'name', label: '姓名', slot: 'name-phone' },
        { prop: 'loginName', label: '登录名' },
        { prop: 'nickName', label: '昵称' },
        { prop: 'avatar', label: '头像', width: 300, slot: 'avatar' },
        { prop: 'age', label: '年龄' },
        { prop: 'status', label: '状态' },
        { prop: 'acorpName', label: '公司名称' },
        { prop: 'departmentName', label: '部门名称' },
        { prop: 'description', label: '描述' },
        { prop: 'time',
          label: '时间',
          children: [
            { prop: 'createTime',
              label: '创建时间',
              children: [
                { prop: 'createTime', label: '创建时间' },
                { prop: 'updateTime', label: '更新时间' }
              ] },
            { prop: 'updateTime', label: '更新时间' }
          ] },
        { label: '操作', slot: 'operator', fixed: 'right', width: 180 }
      ],
      queryParams: {
        keyword: ''
      }
    }
  },
  created () {
    this.queryParams.keyword = 'keyword'
  },
  methods: {
    init () {
      console.log('init方法做一些其它事情')
      this.getTableData()
    },
    getQueryParams () {
      console.log('重写getQueryParams')
      let queryParams = { ...this.queryParams, ...this.pageObj }
      delete queryParams['total']
      return queryParams
    },
    spanMethod ({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 2) {
        if (rowIndex % 2 === 0) {
          return {
            rowspan: 2,
            colspan: 1
          }
        } else {
          return {
            rowspan: 0,
            colspan: 0
          }
        }
      }
    },
    handleView (obj) {
      console.log('查看对象:%o', obj)
    },
    handleDel (obj) {
      console.log('删除对象:%o', obj)
    },
    getTableData () {
      let page = this.pageObj.page
      let limit = this.pageObj.limit
      this.pageObj.total = 100
      this.tableData = []
      for(let i = 0 ; i < limit ; i++){
        this.tableData.push( { id: page*limit + i  } )
      }
    },
    handleSelectionChange (val) {
      console.log(val)
    },
    handleRowClick (row, event, column) {
      console.log('点击行对象:%o', row)
    },
    handleCellClick (row, column, cell, event) {
      console.log('点击单元格对象:%o', cell)
    }
  }
}
</script>
