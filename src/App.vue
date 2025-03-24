<template>
  <div class="InputWrapper">
    <h1>ToDoList</h1>
    <div class="topContent">
      <div class="StarPicker">
        <p class="StarTitle">優先度: </p>
        <StarSelector v-model="selectedStars" />
      </div>
    </div>

    <div class="bottomContent">
      <input class="taskInput" maxlength="15" v-model="taskText" type="text" @keyup.enter="addTask">
      <div class="addBtn" @click="addTask">
        <font-awesome-icon :icon="['fas', 'plus']" size="xl" style="color: white;" />
      </div>
    </div>
  </div>
  <div class="ToDoListWrapper">
    <Task
      v-for="task in taskList"
      :key="task.id"
      :task="task"
      :taskList="taskList"
      @update-task="handleUpdateTask"
      @delete-task="handleDeleteTask"
    />
  </div>
</template>

<script setup>
import { ref, reactive, computed  } from 'vue'
import Task from './components/Task.vue'
import StarSelector from './components/StarSelector.vue'

// 狀態變數
const taskText = ref('')
const selectedStars = ref(1)
const taskList = reactive([])

// 星星選擇
function handleSelectStars(starCount) {
  selectedStars.value = starCount
}

// 新增任務
function addTask() {
  const note = taskText.value.trim()
  const stars = selectedStars.value

  if (!note) return

  const isDuplicate = taskList.some(task => task.note === note)
  if (isDuplicate) {
    taskText.value = ''
    return
  }

  taskList.push({
    id: Date.now(),
    note,
    stars
  })

  // 重置欄位
  taskText.value = ''
  selectedStars.value = 0
}

// 資料更新
function handleUpdateTask(updatedTask) {
  const index = taskList.findIndex(task => task.id === updatedTask.id)
  if (index !== -1) {
    taskList[index] = updatedTask
  }
}

// 資料刪除
function handleDeleteTask(taskId) {
  const index = taskList.findIndex(task => task.id === taskId)
  if (index !== -1) taskList.splice(index, 1)
}
</script>

<style scoped lang="scss">

.InputWrapper{
  width: 600px;
  padding: 0 24px;
  margin-bottom: 12px;

  .topContent{
    display: flex;
    align-content: center;
    justify-content: space-between;
    margin-bottom: 12px;
    .StarPicker{
      display: inline-flex;
      align-items: center;

      .StarTitle{
        margin-right: 12px;
      }
    }
  }
  .bottomContent{
    display: flex;
    justify-content: space-between;
    width: 100%;

    .taskInput{
      display: flex;
      outline: none;
      border: 1px solid gray;
      height: 34px;
      width: 505px;
      box-sizing: border-box;
      border-radius: 4px;
      padding: 0 4px;
    }

    .addBtn{
      width: 75px;
      height: 34px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      background-color: rgb(178, 85, 221);
      cursor: pointer;
      box-sizing: border-box;
      border-radius: 4px;
    }
  }
}

.ToDoListWrapper{
  width: 600px;
  padding: 0 24px;
}

.Star{
  cursor: pointer;
  margin-right: 6px;
}
</style>
