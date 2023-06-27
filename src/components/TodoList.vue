<template>
  <el-row>
    <el-col :span="12" :offset="7" style="width: 100%">
      <h1>TodoList</h1>
      <todo-form @send-message="createTodo"></todo-form>
      <el-table :data="todos">
        <el-table-column prop="title" label="Title" width="410"/>
        <el-table-column fixed="right" label="Operations" width="410">
          <template #default="scope">
            <el-space wrap>
              <el-switch
                  v-model="scope.row.completed"
                  @click="updateTodo(scope.row)"
              />
              <el-popconfirm
                  confirm-button-text="Yes"
                  cancel-button-text="No"
                  icon="el-icon-info"
                  icon-color="red"
                  title="Are you sure?"
                  @confirm="deleteTodo(scope.row)"
                  @cancel="cancelDelete()"
              >
                <template #reference>
                  <el-button size="small" type="danger">Delete</el-button>
                </template>
              </el-popconfirm>
            </el-space>
          </template>
        </el-table-column>
      </el-table>
    </el-col>
  </el-row>
</template>

<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import TodoForm from "./TodoForm.vue";
import {ElMessage, ElTableColumn, ElTable, ElRow, ElCol, ElSpace, ElSwitch} from "element-plus";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

@Options({
  components: {
    TodoForm,
  }
})
export default class TodoList extends Vue {
  todos = [];

  async mounted() {
    await this.loadTodos();
  }

  async loadTodos() {
    const res = await this.axios.get("http://localhost:8081/todos");
    this.todos = res.data;
    console.log(res.data);
  }

  async createTodo(todo: Todo) {
    console.log("Todo", todo);
    await this.axios.post("http://localhost:8081/todos", {
      title: todo.title,
      completed: todo.completed
    });
    ElMessage({
      message: "Todo created",
      type: "success",
    });
    await this.loadTodos();
  }

  async updateTodo(todo: Todo) {
    console.log("Todo", todo);
    await this.axios.put(`http://localhost:8081/todos/${todo.id}`, {
      title: todo.title,
      completed: todo.completed
    })
    ElMessage({
      message: "Todo Updated",
      type: "success",
    });
    await this.loadTodos();
  }

  async deleteTodo(todo: Todo) {
    await this.axios.delete(`http://localhost:8081/todos/${todo.id}`);
    ElMessage({
      message: "Todo Deleted",
      type: "success",
    });
    await this.loadTodos();
  }

  async cancelDelete() {
    console.log("Cancelled the delete");
  }
}
</script>