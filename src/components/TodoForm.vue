<template>
  <el-form @submit.prevent :inline="true" :model="formInput" class="todo-form-input" lang="en">
    <el-form-item label="Title" prop="title">
      <el-input v-model="formInput.title" placeholder="Enter Todo"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">Add</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {ElMessage} from "element-plus";

@Options({})
export default class TodoForm extends Vue {
  formInput = {title: "", completed: false};

  onSubmit() {
    if (this.formInput.title.length > 3) {
      this.$emit("send-message", this.formInput);
    } else {
      ElMessage({
        message: "Warning, Todo must be longer than 3 characters",
        type: "warning"
      });
    }
    this.formInput.title = "";
  }
}
</script>