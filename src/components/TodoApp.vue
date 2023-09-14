<template>
    <div class="container">
        <h2 class="text-center mt-5">My Vue App</h2>
    </div>

    <!-- Input -->
    <div class="d-flex inputBox">
        <input v-model="task" type="text" placeholder="Enter Text" class="form-control">
        <button @click="submitTask" class="btn btn-warning rounded-1 px-5">Submit</button>
    </div>

    <!-- Table -->
    <table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col" class="task-head">Task</th>
      <th scope="col" class="status-head">Status</th>
      <th scope="col" class="text-center fixed-width">Edit</th>
      <th scope="col" class="text-center fixed-width">Delete</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status == 'Finished'}">{{ task.name }}</span>
      </td>
      <td >
        <span @click="changeStatus(index)" class="pointer">
          {{ toCharUpper(task.status) }}
      </span>
    </td>
      <td>
        <div class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
        </div>
      </td> 
      <td>
        <div class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
        </div>
      </td>
 
    </tr>
  </tbody>
</table>
</template>

<script setup> 
import { ref } from 'vue';

let task = ref('');
let editedTask = null;
const availableStatuses = ['to-Do', 'in Progress', 'Finished'];

const tasks = ref([
  {
    name: 'Steal all the items',
    status: 'To-Do',
  },
  {
    name: 'Never again',
    status: 'In Progress',
  },
]);


function submitTask() {
    if (task.value.length === 0) {
        return;
    }
    if(editedTask == null) {
        tasks.value.push(
            {
                name: task.value,
                status: 'To-Do',
            }
            );
    } else {
        tasks.value[editedTask].name = task.value;
        editedTask = null;
    }
    task.value = '';

};  

function deleteTask(index) {
    tasks.value.splice(index, 1);
}

function editTask(index) {
    task.value = tasks.value[index].name;
    editedTask = index;
}

function changeStatus(index) {
    let newIndex = availableStatuses.indexOf(tasks.value[index].status);
    if (++newIndex > 2) {
      newIndex = 0;
    }
    tasks.value[index].status = availableStatuses[newIndex];
}
function toCharUpper(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}
</script>

<style scoped>
.fixed-width {
  width: 10%; 
}
.task-head {
  width: 50%;
}
.status-head {
  width: 20%;
}
.pointer {
  cursor: pointer;
} 
.finished {
  text-decoration: line-through;
}
.inputBox {
  margin-bottom: 1em;
  margin-top: 2em;
}
</style>