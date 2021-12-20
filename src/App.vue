<template>
  <div id="app">
    <header>MINHAS TAREFAS</header>
    <Task></Task>
  </div>
</template>

<script>
import Task from './components/Task.vue';

export default {
  name: "App",
  components:{
    Task
  }
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 80px;
  font-size: 50px;
  color: #fff;
}
</style>
