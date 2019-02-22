<!--suppress ALL -->
<template>
  <!--table模板-->
  <table-layout :show-summary="true" height="500"
                :summary-method="summaryMethod"
                :span-method="spanMethod"
                :pageObj="pageObj"
                :headers="headers"
                :data="tableData"
                @sizeChange="sizeChange"
                @currentChange="currentChange"
                @rowClick="rowClick"
                @cellClick="cellClick"
                @selectionChange="selectionChange">
    <template slot="top-left">
      左边插槽
    </template>
    <template slot="top-right">
      <input v-model="queryParams.keyword"/>
      <input type="button" value="搜索" @click="search"/>
    </template>
    <!---->
    <template slot="expand" slot-scope="{scope}">
          {{scope.row.name}}
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
      <el-button @click.stop="handleView(scope.row)">查看</el-button>
      <el-button @click.stop="handleDel(scope.row)">删除</el-button>
    </template>
  </table-layout>
</template>

<script>
import { Mixins } from '../mixins/mixins'
import TableLayout from './table/TableLayout'
export default {
  name: 'table-demo',
  components: { TableLayout },
  mixins: [ Mixins ],
  data () {
    return {
      headers: [
        { type: 'selection' },
        { type: 'expand', slot: 'expand' },
        { type: 'index', fixed: true },
        { prop: 'id', label: 'id',sortable: true },
        { prop: 'name', label: '姓名', slot: 'name-phone',showOverflowTooltip: true },
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
    summaryMethod(param){
      const { columns, data } = param;
      const sums = [];
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '总价';
          return;
        }
        const values = data.map(item => Number(item[column.property]));
        if (!values.every(value => isNaN(value))) {
          sums[index] = values.reduce((prev, curr) => {
            const value = Number(curr);
            if (!isNaN(value)) {
              return prev + curr;
            } else {
              return prev;
            }
          }, 0);
          sums[index] += ' 元';
        } else {
          sums[index] = '';
        }
      });

      return sums;
    },
    init () {
      console.log('init方法做一些其它事情')
      this.getTableData()
    },
    getQueryParams () {
      let queryParams = { ...this.queryParams, ...this.pageObj }
      delete queryParams['total']
      console.log("搜索条件:%o",queryParams)
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
      let queryParams = this.getQueryParams();
      let page = queryParams.page
      let limit = queryParams.limit
      this.pageObj.total = 100
      this.tableData = []
      for (let i = 0; i < limit; i++) {
        this.tableData.push({ id: page * limit + i, name: '测试' + page * limit + i })
      }
    },
    selectionChange (val) {
      console.log(val)
    },
    rowClick (row, event, column) {
      console.log('点击行对象:%o', row)
    },
    cellClick (row, column, cell, event) {
      console.log('点击单元格对象:%o', cell)
    }
  }
}
</script>
