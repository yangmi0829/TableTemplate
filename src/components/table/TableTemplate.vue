<template>
  <el-table
    :span-method="spanMethod"
    @row-click="handleRowClick"
    @cell-click="handleCellClick"
    @selection-change="handleSelectionChange"
    :data="tableData"
    border
    stripe
    style="width: 100%">
    <template v-for="(i,$index) in header">
      <table-column :i="i" :key="`${$index}`">
        <template v-if="i.slot" :slot="i.slot" slot-scope="{scope}">
          <slot :name="i.slot" :scope="scope">{{i.slot}}</slot>
        </template>
     </table-column>
    </template>
  </el-table>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import TableColumn from './TableColumn.vue'

@Component({
  components: { TableColumn }
})
export default class TableTemplate extends Vue {
  @Prop() spanMethod:Function;
  @Prop() header!:Array<any>;
  @Prop() tableData!:Array<any>;
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
