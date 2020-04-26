<!--需要使用组件实例ref属性只能在父组件定义ref属性，使用this.$refs.<父组件ref属性名>.$children[0]获取-->
<template>
  <el-table
    v-bind="$attrs"
    :data="data"
    :empty-text="emptyText"
    :stripe="stripe"
    class="base-table"
    v-on="$listeners"
  >
    <slot name="font" />

    <template v-for="(column, index) in columns">
      <!--正常表头(不需要处理)-->
      <el-table-column
        v-if="isCommonTableColumn(column) && !column.hidden"
        :key="index"
        align="center"
        v-bind="column.attrs || {}"
      />

      <el-table-column
        v-else-if="!column.hidden"
        :key="index"
        v-bind="column.attrs || {}"
        align="center"
      >
        <template v-slot="scope">
          <span v-if="column.slot">
            <slot :name="column.slot" :scope="scope" />
          </span>

          <!--操作按钮-->
          <div v-else-if="column.operations">
            <template v-for="operation in column.operations">
              <el-button
                :key="operation.text"
                :type="operation.type || 'primary'"
                size="mini"
                :icon="`el-icon-${operation.iconName}`"
                @click.native="handleOperation(operation.event, scope.row)"
              >{{ operation.text }}</el-button>
            </template>
          </div>

          <span v-else>{{ scope.row[column.attr] }}</span>
        </template>
      </el-table-column>
    </template>

    <slot />
  </el-table>
</template>

<script>
import { proxyProp } from '@/utils/proxyProp'
import { findComponentUpwardByProp } from '@/utils/findComponents'

export default {
  name: 'BaseTable',
  props: {
    columns: {
      type: Array,
      required: true
    },
    data: {
      type: Array,
      required: true
    }
  },
  computed: {
    emptyText() {
      return this.$attrs['empty-text'] || '没有符合条件的数据'
    },
    stripe() {
      return this.$attrs.stripe !== false
    }
  },
  mounted() {
    // 代理父组件的columns属性
    const parentComponent = findComponentUpwardByProp(this, 'columns')
    if (parentComponent) {
      parentComponent.columns = parentComponent.columns.map(column =>
        proxyProp(column)
      )
    } else {
      throw new Error('can not find parentComponent')
    }
  },
  methods: {
    // 是否是一个常规的table-column(有以下标签就不是常规table-column)
    isCommonTableColumn(column) {
      const specialColumnList = ['slot', 'operations']
      return !specialColumnList.some(option => column[option])
    },
    // 点击操作按钮触发的事件
    handleOperation(event, row) {
      console.log(event)
      this.$emit(event, row)
    }
  }
}
</script>
