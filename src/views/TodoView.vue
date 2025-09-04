<template>
<!-- ToDo List -->
<div id="todoListPage" class="bg-half">
  <nav>
    <h1><a href="#">ONLINE TODO LIST</a></h1>
    <ul>
      <li class="todo_sm"><a href="#"><span>王小明的代辦</span></a></li>
      <li><RouterLink to="/login" class="formControls_btnLink">登出</RouterLink></li>
    </ul>
  </nav>
  <div class="conatiner todoListPage vhContainer">
    <div class="todoList_Content">
      <div class="inputBox">
        <input type="text" placeholder="請輸入待辦事項" v-model="postTodoField.content">
        <a href="#" @click="postTodo()">
          <i class="fa fa-plus"></i>
        </a>
      </div>
      <div class="todoList_list">
        <ul class="todoList_tab">
          <li><a href="#" @click="changeTabMode('all')" :class="pagetabMode=='all'?'active':''">全部</a></li>
          <li><a href="#" @click="changeTabMode('pending')" :class="pagetabMode=='pending'?'active':''">待完成</a></li>
          <li><a href="#" @click="changeTabMode('completed')" :class="pagetabMode=='completed'?'active':''">已完成</a></li>
        </ul>
        <TodoList :todoList="pageTodoList" :completedCount="pageCompletedCount" :incompleteCount="pageIncompleteCount" :tabMode="pagetabMode" @emit-status-checkbox="changeTodoStatus" @emit-del-checkbox="deleteTodo" />
      </div>
    </div>
  </div>
</div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'
import TodoList from '../components/TodoList.vue'

const pageCompletedCount = computed(()=> {
  const count = pageTodoList.value.filter(item => item.status == true).length
  return count
})
const pageIncompleteCount = computed(()=> {
  const count = pageTodoList.value.filter(item => item.status == false).length
  return count
})

const pagetabMode = ref('all')
const changeTabMode = (mode) => {
  pagetabMode.value = mode
}

onMounted(async() => {
  // checkUser()
  getTodo()
})

const apiBaseUrl = 'https://todolist-api.hexschool.io/';
const token = document.cookie.replace(/(?:^|.*;\s*)userToken\s*=\s*([^;]*).*$/i,"$1")
// 使用者驗證
const checkUser = async() => {
  try{
    const res = await axios.get(`${apiBaseUrl}users/checkout`,{
        headers:{
            Authorization: token
        }
    });
    if (res.data.status) {
      alert("使用者驗證成功");
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

// 取得代辦項目
const getTodo = async() => {
  try{
    const res = await axios.get(`${apiBaseUrl}todos/`,{
      headers: {
        Authorization: token
      }
    });
    if (res.data.status) {
      pageTodoList.value = res.data.data;
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

const postTodoField = ref({
  content: ''
})
// 新增代辦項目
const postTodo = async() => {
  try{
    const res = await axios.post(`${apiBaseUrl}todos/`,
    { content: postTodoField.value.content },
    {
        headers:{
            Authorization: token
        }
    });
    if (res.data.status) {
      alert("新增代辦項目成功");
      getTodo()
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

// 更新代辦項目內容
const putTodo = async(id) => {
  try{
    const res = await axios.put(`${apiBaseUrl}todos/${id}`,{
        headers:{
            Authorization: token
        }
    });
    if (res.data.status) {
      alert("更新代辦項目內容成功");
      getTodo()
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

// 刪除代辦項目
const deleteTodo = async(id) => {
  try{
    const res = await axios.delete(`${apiBaseUrl}todos/${id}`,{
        headers:{
            Authorization: token
        }
    });
    if (res.data.status) {
      alert("刪除代辦項目成功");
      getTodo()
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

let isChanging = false
const changeTodoStatus = (id) => {
  if (isChanging) return; // 阻擋連點
  isChanging = true;
  setTimeout(() => {
    patchTodo(id)
    isChanging = false; // 執行完成後才允許再次點擊
  },1000)
}
// 更新代辦項目狀態
const patchTodo = async(id) => {
  try{
    const res = await axios.patch(`${apiBaseUrl}todos/${id}/toggle`,
    {}, 
    {
        headers:{
            Authorization: token
        }
    });
    if (res.data.status) {
      alert("更新代辦項目狀態成功");
      getTodo()
    }
  }catch(error){
    const errorMsg = error.response.data && error.response.data.message ? error.response.data.message : '未知的錯誤';
    alert(errorMsg);
  }
}

const pageTodoList = ref([])
</script>