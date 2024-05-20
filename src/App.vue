<script setup>
import { onMounted,ref } from 'vue';
import Edit from './components/Edit.vue'
import axios from 'axios';

// TODO: 列表渲染 思路：声明响应式list→调用接口获取数据→后端数据赋值给list→绑定到table组件

const list = ref([])
const getlist = async()=>{
  //接口调用
  const res = await axios.get('/list')
  //交给list
  list.value = res.data
}

onMounted(()=>getlist())//在onMounted中调用getlist方法
// TODO: 删除功能 思路：获取当前行的id→通过id调用删除接口→更新最新的列表
//1.准备一个删除的回调函数
const onDelete = async(id) =>{
  
  await axios.delete(`/del/${id}`)
  getlist()
}

// TODO: 编辑功能

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{row}">
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
