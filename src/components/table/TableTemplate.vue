<template>
  <el-table
    @row-click="rowClick"
    @cell-click="cellClick"
    @selection-change="selectionChange"
    v-bind="$attrs"
    border
    stripe
    style="width: 100%">
    <template v-for="(i,$index) in headers">
      <table-column :col="i" :key="`${$index}`">
        <template v-if="i.slot" :slot="i.slot" slot-scope="{scope}">
          <slot :name="i.slot" :scope="scope">{{i.slot}}</slot>
        </template>
     </table-column>
    </template>
  </el-table>
</template>

<script>
import TableColumn from './TableColumn.vue'
export default {
  name: 'table-template',
  props: {
    headers: { type: Array, required: true }
  },
  components: { TableColumn },
  methods: {
    selectionChange (val) {
      this.$emit('selectionChange', val)
    },
    rowClick (row, event, column) {
      this.$emit('rowClick', row, event, column)
    },
    cellClick (row, column, cell, event) {
      this.$emit('cellClick', row, column, cell, event)
    }
  }
}
</script>
