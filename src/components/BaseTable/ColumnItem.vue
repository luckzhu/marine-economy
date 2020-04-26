<!-- <template>-->
  <!-- <div> -->
    <!-- hack 为了列顺序正确，循环产生的el-table-colmun的第一列会到最后一列-->
    <!-- <el-table-column width="1" />
    <el-table-column v-for="(column,index) in item" :key="(column.label+index)" :prop="column.prop" :label="column.label" :width="column.width?column.width:'auto'" align="center" :formatter="column.formatter" :fixed="column.fixed" :class-name="column.prop" >
      <template v-if="column.children">
        <column-item :item="column.children"></column-item>
      </template>
    </el-table-column> -->
  <!-- </div> -->
<!--</template>-->

<script>
export default {
  name: 'ColumnItem',
  props: {
    item: {
      type: Array,
      required: true
    }
  },
  computed: {
    // 由于el-table的columns列的排序错误bug,第一项会排到最后一列
    // 所以提前将最后一项移到第一项
    hackItems() {
      const { item } = this
      const temp = item.slice(0, -1)
      temp.unshift(item[item.length - 1])
      return temp
    }
  },
  render(h, context) {
    const { hackItems } = this
    return h(
      'template',
      hackItems.map((column, index) => {
        // console.log(column)
        return h(
          'el-table-column',
          {
            attrs: {
              key: column.label + index,
              prop: column.prop,
              label: column.label,
              width: column.width ? column.width : 'auto',
              align: 'center',
              formatter: column.formatter,
              fixed: column.fixed,
              className: column.prop
            }
          },
          [
            this.$slots.default,
            (function() {
              if (column.children) {
                return h('column-item', {
                  attrs: {
                    item: column.children
                  }
                })
              }
            })()
          ]
        )
      })
    )
  }
}
</script>

