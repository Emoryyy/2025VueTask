<template>
    <div class="todoList_items">
        <ul class="todoList_item">
        <li v-for="item in filteredTodoList" :key="item.id">
            <label class="todoList_label">
            <input class="todoList_input" type="checkbox" value="true" :checked="item.status" @change="changeStatus(item.id)">
            <span>{{ item.content }}</span>
            </label>
            <a href="#" @click="delItem(item.id)">
            <i class="fa fa-times"></i>
            </a>
        </li>
        </ul>
        <div class="todoList_statistics">
            <p v-if="filteredTodoList.length>0">{{ props.incompleteCount }} 個待完成項目</p>
            <p v-if="filteredTodoList.length>0">{{ props.completedCount }} 個已完成項目</p>
            <p v-else>目前尚無待辦事項</p>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
    todoList: { type: Array, required: true },
    completedCount: { type: Number, required: true },
    incompleteCount: { type: Number, required: true },
    tabMode: { type: String, required: false }
})

const emit = defineEmits(['emit-status-checkbox'],['emit-del-checkbox'])
const changeStatus = (productId) => {
  emit('emit-status-checkbox', productId)
}
const delItem = (productId) => {
  emit('emit-del-checkbox', productId)
}

const filteredTodoList = computed(() => {
    if(props.tabMode == 'completed') {
        return props.todoList.filter(item => item.status==true) 
    }
    else if(props.tabMode == 'pending') {
        return props.todoList.filter(item => item.status==false)
    }
    return props.todoList
})
</script>