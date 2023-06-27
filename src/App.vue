<template>
  <el-container>
    <el-main>
      <todo-list></todo-list>
    </el-main>
  </el-container>
</template>

<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import TodoList from './components/TodoList.vue';
import {ElContainer, ElMain} from 'element-plus';

@Options({
  components: {
    TodoList,
    ElContainer,
    ElMain,
  },
})
export default class App extends Vue {
}
</script>

<style>
body {
  margin: 0;
}
.el-container {
  background-color: #e9eef3;
  color: var(--el-text-color-primary);
  text-align: center;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
