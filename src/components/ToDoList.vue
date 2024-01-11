<script setup lang="ts">
import { computed, ref } from 'vue'

interface Task{
  name: string
  isFinished: boolean
}

const tasks = ref<Task[]>([
  { name: '部屋の片付け', isFinished: false },
  { name: 'なろう講習会', isFinished: false },
])

const finishedTasks = computed(() => tasks.value.filter((task) => task.isFinished))

const notFinishedTasks = computed(() => tasks.value.filter((task) => !task.isFinished))

const newTaskName = ref('')

const addTask = () => {
  if (newTaskName.value === '') {
    alert('タスク名を入力してください')
    return
  }if (tasks.value.some((task) => task.name === newTaskName.value)) {
    alert('同じ名前のタスクがあります')
    return
  }
  tasks.value.push({
    name: newTaskName.value,
    isFinished: false
  })
  newTaskName.value = ''
}

const finishTask = (taskName: string) => {
  tasks.value = tasks.value.map((task) => {
    if (task.name === taskName) {
      return {
        ...task,
        isFinished: true
      }
    }
    return task
  })
}
</script>

<template>
  <div>
    <div>ToDoList</div>
    <div>完了済みタスク一覧</div>
    <ul>
      <li v-for="task in finishedTasks" :key="task.name">
        <div>{{ task.name }}</div>
      </li>
    </ul>
    <div>未完タスク一覧</div>
    <ul>
      <li v-for="task in notFinishedTasks" :key="task.name">
        <div>{{ task.name }}</div>
        <div>
          <button @click="finishTask(task.name)">完了！</button>
        </div>
      </li>
    </ul>
    <div>
      <label>
        名前
        <input v-model="newTaskName" type="text" />
      </label>
      <button @click="addTask">追加</button>
    </div>
  </div>
</template>