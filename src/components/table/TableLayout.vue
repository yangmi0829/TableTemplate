<template>
    <div class="box">
      <!--顶部搜索按钮-->
      <div class="top">
        <div  class="left">
          <slot name="top-left">
          </slot>
        </div>
        <div class="right">
          <slot class="right" name="top-right">
          </slot>
        </div>
      </div>
      <!--table-->
      <div class="table">
        <table-template
          v-bind="$attrs"
          v-on="$listeners"
          :headers="headers"
          ref="table">
          <template  v-for="i in headers"  :slot="i.slot" slot-scope="{scope}">
              <slot v-if="i.slot" :name="i.slot" :scope="scope">{{i.slot}}</slot>
            </template>
        </table-template>
      </div>
      <!--分页-->
      <div class="pagination" style="text-align: right;margin-top: 10px">
        <slot name="pagination">
          <el-pagination
            @size-change="sizeChange"
            @current-change="currentChange"
            layout="sizes, prev, pager, next, jumper, ->, total, slot"
            :total="pageObj.total" :page-size="pageObj.limit"
            :current-page="pageObj.page">
          </el-pagination>
        </slot>
      </div>
    </div>
</template>

<script>
import TableTemplate from './TableTemplate.vue'
export default {
  name: 'table-layout',
  props: {
    headers: { type: Array, required: true },
    pageObj: { type: Object, required: true }
  },
  components: { TableTemplate },
  methods: {
    // 分页长度改变
    sizeChange (val) {
      this.$emit('sizeChange', val)
    },
    // 分页页码改变
    currentChange (val) {
      this.$emit('currentChange', val)
    }
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
