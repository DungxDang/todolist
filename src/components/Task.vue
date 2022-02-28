<script>
export default {
  name: "TaskToDo",
  props: ["taskName", "isComplete", "idTask", "select"],
  emits: ["deleteTask", "toggleTask"],
  watch: {
    check(newValue) {
      this.$emit('toggleTask', this.idTask, newValue)
    },
  },
  data() {
    return {
      check: false,
    };
  },
  mounted(){
  }
};
</script>

<template>
  <div class="wrap">
    <div class="checkbox">
      <input type="checkbox" v-model="check" :disabled="select==='all' ? !isComplete : false"/>
    </div>
    <div :class="['content', {completed: !isComplete}]"><slot/></div>
    <div class="close" @click="$emit('deleteTask', idTask)">+</div>
  </div>
  <svg
    viewBox="0 0 405.272 405.272"
    v-if="false"
  >
    <g>
      <path
        d="M393.401,124.425L179.603,338.208c-15.832,15.835-41.514,15.835-57.361,0L11.878,227.836   c-15.838-15.835-15.838-41.52,0-57.358c15.841-15.841,41.521-15.841,57.355-0.006l81.698,81.699L336.037,67.064   c15.841-15.841,41.523-15.829,57.358,0C409.23,82.902,409.23,108.578,393.401,124.425z"
      />
    </g>
  </svg>
</template>

<style scoped>
.wrap {
  /* width: 100%; */
  display: flex;
  align-items: center;
  position: relative;
  border: solid 0.5px #cccccc;
  height: 36px;
}
.checkbox {
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 10px;
}
.checkbox > input {
  width: 20px;
  height: 20px;
}
.content {
  width: calc(100% - 50px);
  font-size: 16px;
  line-height: 16px;
  text-align: left;
  padding-left: 20px;
  color: rebeccapurple;
}
.completed{
  color: green;
}
.close {
  position: absolute;
  right: 10px;
  color: red;
  font-size: 32px;
  transform: rotate(45deg);
  cursor: pointer;
}
</style>
