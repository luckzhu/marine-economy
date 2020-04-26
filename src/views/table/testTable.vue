<template>
  <div class="app-container">
    <base-table :data="list" :columns="columns">
      <template v-slot:testSlot="{ scope }">
        <el-tag>{{ scope.row.pageviews }}</el-tag>
      </template>
    </base-table>
  </div>
</template>

<script>
import { getList } from '@/api/table'

export default {
  components: {
    BaseTable: () => import('@/components/BaseTable/index')
  },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: [],
      listLoading: true,
      columns: [
        {
          attrs: {
            prop: 'title',
            label: 'Title',
            align: 'left'
          }
        },
        {
          attrs: {
            prop: 'author',
            label: 'Author'
          }
        },
        {
          attrs: {
            label: '操作'
          },
          operations: [
            {
              tipContent: '图标按钮提示信息',
              svgName: 'user',
              className: 'user',
              event: function(row) {
                console.log(row)
              }
            },
            {
              tipContent: '图标按钮提示信息',
              svgName: 'logout',
              className: 'logout',
              event: 'printRow'
            }
          ]
        },
        {
          slot: 'testSlot',
          attrs: {
            prop: 'pageviews',
            label: 'Display_time'
          }
        }
      ]
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      getList().then(response => {
        this.list = response.data.items
        this.listLoading = false
      })
    },
    printRow(row) {
      console.log(row)
    }
  }
}
</script>
