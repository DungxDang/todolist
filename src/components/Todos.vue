<script>
import Task from "./Task.vue";

export default {
  name: "ToDoList",
  components: {
    Task,
  },
  props: {
    msg: String,
  },
  setup() {
    return {
      dataRaw: [
        {
          idTask: "1",
          content: "Learn Vuejs",
          isComplete: true,
        },
        {
          idTask: "2",
          content: "Init Project",
          isComplete: true,
        },
        {
          idTask: "3",
          content: "Finish development",
          isComplete: false,
        },
      ],
    };
  },
  data() {
    return {
      check: false,
      data: this.dataRaw,
      checkTasks: {},
      taskName: "",
      select: "all",
      editId: false,
    };
  },
  computed: {
    tasks() {
      return this.filter(this.select);
    },
    itemleft() {
      let checked = 0;
      for (let key in this.checkTasks) if (this.checkTasks[key]) checked++;
      return this.tasks.length - checked;
    },
  },
  methods: {
    deleteTask(idTask) {
      let index = 0;
      this.data.forEach((task, i) => {
        if (task.idTask === idTask) {
          index = i;
          delete this.checkTasks[idTask];
        }
      });
      this.data.splice(index, 1);
    },
    toggleTask(idTask, isCheck) {
      this.checkTasks[idTask] = isCheck;
    },
    moveToComplete(idTask) {
      this.data.forEach((task) => {
        if (task.idTask === idTask) task.isComplete = true;
      });
    },
    deleteTasks() {
      if (this.select === "active")
        for (let key in this.checkTasks) {
          if (this.checkTasks[key]) this.moveToComplete(key);
        }
      else {
        const newData = [];
        this.data.forEach((task) => {
          if (!this.checkTasks[task.idTask]) newData.push(task);
        });
        this.data = newData;
      }
      this.checkTasks = {};
    },
    complete(idTask) {
      const newData = [...this.data];
      for (let task of newData) {
        if (task.idTask === idTask) {
          task.isComplete = true;
          break;
        }
      }
      this.data = newData;
    },
    addTask() {
      if (this.editId) {
        this.data.forEach((task) => {
          if (task.idTask === this.editId) {
            task.content = this.taskName;
            this.editId = "";
          }
        });
      } else
        this.data.push({
          idTask: Math.random() + "",
          content: this.taskName,
          isComplete: false,
        });
      this.taskName = "";
    },
    filter(select) {
      const array = [];
      if (select === "active")
        this.data.forEach((task) => {
          if (!task.isComplete) array.push(task);
        });
      else if (select === "completed")
        this.data.forEach((task) => {
          if (task.isComplete) array.push(task);
        });
      else return this.data;
      return array;
    },
    editTaskName(idTask, nameEdit) {
      this.taskName = nameEdit;
      this.editId = idTask;
    },
    closeEdit() {
      this.taskName = "";
      this.editId = "";
    },
    selectFilter(select) {
      this.select = select;
      this.checkTasks = {};
    },
  },
};
</script>

<template>
  <div class="wrap">
    <div class="ad-task">
      <input
        type="text"
        v-model="taskName"
        :placeholder="editId ? 'Edit task name' : `What's need to be done?`"
        @keyup.enter="addTask"
      />
      <div class="close" @click="closeEdit" v-if="!!editId">+</div>
    </div>
    <div class="stroke">
      <div></div>
      <div></div>
    </div>
    <div class="select">
      <div>{{ itemleft }} item lefts</div>
      <div>
        <div
          @click="selectFilter('all')"
          :class="{ selected: select === 'all' }"
        >
          ALL
        </div>
        <div
          @click="selectFilter('active')"
          :class="{ selected: select === 'active' }"
        >
          ACTIVE
        </div>
        <div
          @click="selectFilter('completed')"
          :class="{ selected: select === 'completed' }"
        >
          COMPLETED
        </div>
      </div>
    </div>
    <Task
      v-for="task of tasks"
      :key="task.idTask"
      :idTask="task.idTask"
      :isComplete="task.isComplete"
      :select="select"
      @deleteTask="deleteTask"
      @toggleTask="toggleTask"
    >
      <div
        :class="['task-name', { edit: task.idTask === editId }]"
        @click="editTaskName(task.idTask, task.content)"
      >
        {{ task.content }}
      </div>
    </Task>
    <div @click="deleteTasks" class="btn">
      {{ select === "active" ? "Move to complete" : "Clear Completed" }}
    </div>
  </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrap {
  width: 500px;
}
.ad-task {
  position: relative;
}
.ad-task > input {
  width: calc(100% - 67px);
  /* width: inherit; */
  height: 36px;
  outline: none;
  border: solid 0.5px #cccccc;
  padding-left: 63px;
  font-size: 16px;
  line-height: 16px;
  margin-bottom: 20px;
}
.stroke {
  display: flex;
  /* width: calc(100% + 1.3px); */
}
.stroke > div:first-child {
  width: 70%;
  height: 10px;
  background-color: rebeccapurple;
  opacity: 0.9;
}
.stroke > div:last-child {
  width: 30%;
  height: 10px;
  background-color: rebeccapurple;
  opacity: 0.4;
}
.btn {
  border-radius: 100px;
  /* width: calc(100% + 1.3px); */
  height: 36px;
  background: rebeccapurple;
  opacity: 0.9;
  margin-top: 20px;
  font-size: 16px;
  line-height: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  cursor: pointer;
}
.select {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 36px;
  /* width: calc(100% + 1.3px); */
  font-size: 16px;
  line-height: 16px;
  border: solid 0.5px #cccccc;
  color: rebeccapurple;
  padding-left: 15px;
}
.select > div:last-child {
  display: flex;
}
.select > div:last-child div {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 26px;
  padding: 0px 10px;
  cursor: pointer;
  /* color: #663399; */
  border-radius: 2px;
}
.selected {
  background: #66339930;
}
.task-name {
  cursor: pointer;
  width: fit-content;
}
.edit {
  color: red;
}
.close {
  position: absolute;
  right: 10px;
  top: 3px;
  color: red;
  font-size: 32px;
  transform: rotate(45deg);
  cursor: pointer;
}
</style>
