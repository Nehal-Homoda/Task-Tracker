<template>
  <header>
    <h1>{{ title }}</h1>
    <Button @toggle-add-task="$emit('toggle-add-task')" :text="showAddTask ?'close':'Add Task'" :color="showAddTask ?'red':'green'" />
    <Button text="Update Task" color="blue"/>
    <Button text="Delete Task" color="red"/>
  </header>
</template>

<script>
import Button from "./Button.vue";
export default {
  name: "Header",
  props: {
    title: String,
    showAddTask:Boolean
  },
  components: { Button },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
