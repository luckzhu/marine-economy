<template>
  <div class="monitoring">
    <base-table :data="tableData" :columns="columns" @toEdit="toEdit" @toCheck="toCheck" />
  </div>
</template>

<script>
export default {
  name: 'Monitoring',
  components: {
    BaseTable: () => import('@/components/BaseTable/index')
  },
  data() {
    return {
      tableData: [
        {
          id: 12687,
          year: 2020,
          month: 5,
          reportName: '海洋经济企业月度统计表',
          pdfUrl: '',
          taskState: 0,
          unitName: '江门市鹏中皇畜牧有限公司',
          state: 9,
          endTime: 1517500800000,
          seq: 40
        }
      ],
      columns: [
        {
          attrs: {
            label: '统计年份',
            prop: 'year'
          }
        },
        {
          attrs: {
            label: '统计月份',
            prop: 'month'
          }
        },
        {
          attrs: {
            label: '报表名称',
            prop: 'reportName'
          }
        },
        {
          attrs: {
            label: '统计状态',
            prop: 'state',
            formatter(row, column, cellValue) {
              return cellValue === 0 ? '未操作' : '市级审核通过'
            }
          }
        },
        {
          attrs: {
            label: '截止时间',
            prop: 'endTime'
          }
        },
        {
          attrs: {
            label: '审核意见'
          },
          operations: [
            {
              text: '查看',
              type: 'success',
              iconName: 'document-checked',
              event: 'toCheck'
            }
          ]
        },
        {
          attrs: {
            label: '操作'
          },
          operations: [
            {
              text: '填报',
              iconName: 'edit',
              event: 'toEdit'
            }
          ]
        }
      ]
    }
  },
  computed: {
    name() {
      return this.$route.name
    }
  },
  methods: {
    toEdit(row) {
      console.log(row)
    },
    toCheck(row) {
      console.log(row)
    }
  }
}
</script>

<style lang="scss" scoped>
.monitoring {
  padding: 20px;
}
</style>
