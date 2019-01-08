<template>
    <div class="box">
      <!--顶部搜索按钮-->
      <div class="top">
        <div  class="left">
          <slot name="top-left">
            <el-button size="mini">左边按钮</el-button>
          </slot>
        </div>
        <div class="right">
          <slot class="right" name="top-right">
            <el-button size="mini" @click="search">右边按钮</el-button>
          </slot>
        </div>
      </div>
      <!--table-->
      <div class="table">
        <table-template @handleRowClick="handleRowClick" @handleCellClick="handleCellClick" :spanMethod="spanMethod" :header="header" ref="table" :tableData="tableData" @selectionChange="handleSelectionChange">
          <template  v-for="i in header"  :slot="i.slot" slot-scope="{scope}">
              <slot v-if="i.slot" :name="i.slot" :scope="scope">{{i.slot}}</slot>
            </template>
        </table-template>
      </div>
      <!--分页-->
      <div class="pagination" style="text-align: right;margin-top: 10px">
        <slot name="pagination">
          <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" layout="sizes, prev, pager, next, jumper, ->, total, slot" :total="pageObj.total" :page-size="pageObj.limit" :current-page="pageObj.page">
          </el-pagination>
        </slot>
      </div>
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import PageMixins, { Page } from '../../mixin/mixin'
import TableTemplate from './TableTemplate.vue'

@Component({
  components: { TableTemplate },
  mixins: [PageMixins]
})
export default class TableLayout extends Vue {
  method:string = 'emitEvent';
  @Prop() spanMethod:any;
  @Prop() header!:Array<object>;
  @Prop() tableData!:Array<any>;
  @Prop()pageObj!:Page;
  emitEvent () {
    let method = ''
    if (this.$parent) {
      // @ts-ignore
      method = this.$parent.method
    }
    this.$emit(method || this.method, this.pageObj)
  }
  handleSelectionChange (val:any) {
    this.$emit('selectionChange', val)
  }
  handleRowClick (row:any, event:any, column:any) {
    this.$emit('handleRowClick', row, event, column)
  }
  handleCellClick (row:any, column:any, cell:any, event:any) {
    this.$emit('handleCellClick', row, column, cell, event)
  }
}
</script>

<style lang="scss" scoped>
  .box{
    .top-content,.middle-content{
        background-color: white;
        width: 100%;
        margin: 10px 0;
        padding: 10px;
      }
  }
  .table{margin-top: 10px}
.top{
  overflow: hidden;
  padding:10px 0;
  background-color: white;
  &:after{
    content: "";
    clear: both;
  }
  .left{
    float: left;
    line-height: 28px;
  }
  .right{float: right}
}
</style>
