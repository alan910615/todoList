<template>
  <div class="task">
    <input
      v-if="isEditing"
      class="note-input"
      type="text"
      maxlength="15"
      v-model="task.note"
      placeholder="輸入任務內容"
    />
    <p v-else class="note">{{ task.note }}</p>

    <!-- 星星選擇 -->
    <StarSelector
      v-if="isEditing"
      v-model="task.stars"
    />
    <p v-else class="stars">⭐ {{ task.stars }} 星</p>

    <!-- 資料刪除 -->
    <div v-if="isEditing" class="delete-btn">
      <font-awesome-icon :icon="['fas', 'trash']" size="lg" style="color: white;" @click="onDelete(task.id)" />
    </div>

    <!-- 編輯/完成 按鈕 -->
    <div
      class="edit-btn"
      :class="{ editing: !isEditing }"
      @click="onToggleEditing"
      aria-label="切換編輯模式"
    >
      <font-awesome-icon
        :icon="isEditing ? ['fas', 'check'] : ['fas', 'pen-to-square']"
        size="lg"
        style="color: white;"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import StarSelector from './StarSelector.vue'

// Props & Emits
const props = defineProps({
  task: {
    type: Object,
    required: true
  },
  taskList: {
    type: Array,
    required: true
  }
})
const emit = defineEmits(['update-task', 'delete-task'])

// 編輯狀態
const isEditing = ref(false)

// 切換編輯狀態
function onToggleEditing() {
  isEditing.value = !isEditing.value

  // 當關閉編輯時，檢查是否重複
  if (!isEditing.value) {
    const trimmedNote = props.task.note.trim()

    const isDuplicate = props.taskList.some(t =>
      t.id !== props.task.id && t.note.trim() === trimmedNote
    )

    if (isDuplicate) {
      alert('已有相同的任務名稱，請重新輸入！')
      isEditing.value = true // 留在編輯狀態
      return
    }

    // 發送更新事件
    emit('update-task', props.task)
  }
}

// 資料刪除

function onDelete (id) {
  emit('delete-task', id)
}
</script>


<style scoped lang="scss">
.task {
  width: 100%;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  border-bottom: 1px solid #a8a8a8;
  margin-bottom: 4px;
  position: relative;

  .note-input,
  .note {
    width: 200px;
    height: 34px;
    box-sizing: border-box;
    padding: 0 4px;
    position: absolute;
    left: 0;
    display: inline-flex;
    align-items: center;
    border-radius: 4px;
  }

  .note-input {
    outline: none;
    border: 1px solid gray;
  }

  .note {
    border: none;
  }

  .stars {
    margin-left: 220px;
  }

  .edit-btn {
    width: 50px;
    height: 36px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    margin-left: 20px;
    background-color: rgb(0, 182, 0);
    cursor: pointer;

    &.editing {
      background-color: rgb(255, 59, 59);
    }
  }

  .delete-btn{
    width: 50px;
    height: 36px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    margin-left: 20px;
    background-color: rgb(255, 59, 59);
    cursor: pointer;
  }
}
</style>
