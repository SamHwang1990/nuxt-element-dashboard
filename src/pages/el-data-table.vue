<template>
  <el-data-table v-bind="tableConfig"></el-data-table>
</template>

<script>
import {formatDate} from '@/const/filter'
import CATEGORIES from '@/const/component-categories'
import STATUS from '@/const/component-status'

export default {
  data() {
    const $createElement = this.$createElement.bind(this)

    const categoriesSearchOptions = Object.keys(CATEGORIES).reduce((
      result,
      key
    ) => {
      result.push({
        label: CATEGORIES[key],
        value: key
      })
      return result
    }, [])

    const statusSearchOptions = Object.keys(STATUS).reduce((result, key) => {
      result.push({
        label: STATUS[key],
        value: key
      })
      return result
    }, [])

    return {
      tableConfig: {
        url: '/hiring/components',
        columns: [
          {prop: 'id', type: 'selection'},
          {prop: 'name', label: '组件名称'},
          {
            prop: 'category',
            label: '分类',
            formatter(row, column, cellValue, index) {
              return CATEGORIES[cellValue]
            }
          },
          {prop: 'version', label: '版本'},
          {prop: 'language', label: '开发语言'},
          {
            prop: 'updateTime',
            label: '最后更新时间',
            formatter(row, column, cellValue, index) {
              return formatDate(parseInt(cellValue), 'YYYY-MM-DD')
            }
          },
          {
            prop: 'status',
            label: '状态',
            formatter(row, column, cellValue) {
              return $createElement(
                'span',
                {
                  class: {
                    'cell-status-stale': cellValue === 0,
                    'cell-status-active': cellValue === 1
                  }
                },
                [STATUS[cellValue]]
              )
            }
          }
        ],
        totalPath: 'payload.total',
        searchForm: [
          {
            $el: {
              placeholder: '请输入',
              suffixIcon: 'el-icon-tickets'
            },
            label: '组件名称',
            $id: 'name',
            $type: 'input'
          },
          {
            $el: {placeholder: '请选择'},
            label: '分类',
            $id: 'category',
            $type: 'select',
            $options: categoriesSearchOptions
          },
          {
            $el: {placeholder: '请选择'},
            label: '状态',
            $id: 'status',
            $type: 'select',
            $options: statusSearchOptions
          }
        ],
        hasEdit: false,
        operationAttrs: {
          width: 210
        },
        extraButtons: [
          {
            type: 'primary',
            size: 'small',
            text: '查看',
            atClick: row => {
              alert('查看' + row.name)
              return Promise.resolve(false)
            }
          },
          {
            text: '编辑',
            size: 'small',
            atClick: row => {
              alert('编辑' + row.name)
              return Promise.resolve(false)
            }
          },
          {
            text: '上架',
            size: 'small',
            show: row => row.status === 0,
            atClick: row => {
              alert('上架' + row.name)
              return Promise.resolve(false)
            }
          },
          {
            text: '下架',
            size: 'small',
            show: row => row.status === 1,
            atClick: row => {
              alert('下架' + row.name)
              return Promise.resolve(false)
            }
          }
        ],
        async onNew(data) {
          alert('custom on new ')
          return true
        },
        async onDelete(data) {
          alert('custom on delete')
          return true
        }
      }
    }
  }
}
</script>

<style>
.cell-status-stale {
  color: gray;
}

.cell-status-active {
  color: green;
}
</style>
