<template>
  <el-table :data="items" style="width: 80%" @sortChange="handleSortChange">
    <el-table-column label="아이디" prop="id" sortable="custom"></el-table-column>
    <el-table-column label="이름" prop="title" sortable="custom" />
    <el-table-column label="내용" prop="content" sortable="custom"/>
  </el-table>
  <el-pagination background layout="prev, pager, next" :total="pagination.totalRowCount" :pageSize="pagination.pageSize" v-model="pagination.currentPage" @currentChange="onPageChanged"/>
</template>

<script>
// 만약 reactive가 vue버전에 호환되지 않는다면, reference를 사용할 것!
import { defineComponent, ref, reactive, computed, onMounted } from 'vue'
// element components
import { ElTable, ElTableColumn, ElPagination } from 'element-plus'
// css list
import 'element-plus/es/components/table/style/css'
import 'element-plus/es/components/table-column/style/css'
import 'element-plus/es/components/pagination/style/css'

export default defineComponent({
  components: {
    ElTable,
    ElTableColumn,
    ElPagination
  },
  setup() {
    let items = reactive([]);
    const pagination = {
      pageSize: 10,
      currentPage: 1,
      totalRowCount: 100
    }
    

    const getItems = () => {
      // TODO 백엔드 호출 페이지네이션, 소팅에 맞는 데이터 조회하기!
      items.length = 0
      const { currentPage, pageSize } = pagination
      const startIndex = (currentPage - 1) * pageSize
      const endIndex = startIndex + pageSize
      
      for(let i = startIndex ; i < endIndex; i ++) {
        items.push({
          id: i,
          title: `어떤 제목 - #${i}`,
          content: `어떤 내용이에요 - #${i}`,
        })
      }
    }

    const onPageChanged = (page) => {
      alert(`현재 페이지: ${page}`)
      pagination.currentPage = page
      getItems()
    }

    onMounted(() => {
      onPageChanged(1)
    })    

    return {
      items,
      handleSortChange: ({ prop, order }) => { // 정렬 기능
        alert(`정렬-키: ${prop}, 정렬-순서: ${order}`);
      },

      pagination: ref(pagination),
      onPageChanged // 페이지네이션 호출 기능
    }
  }
})
</script>

