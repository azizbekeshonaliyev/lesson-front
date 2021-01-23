<template>
  <el-container>
    <el-main>
      <el-table :data="tableData" style="width: 100%" v-loading="loading">
        <el-table-column prop="id" label="Id" width="180"> </el-table-column>
        <el-table-column prop="title" label="Title" width="180">
        </el-table-column>
        <el-table-column prop="body" label="Body"> </el-table-column>
        <el-table-column prop="created_at" label="Created"> </el-table-column>
      </el-table>
      <el-row class="mt-2">
        <el-form ref="form" :model="form" label-width="120px">
          <el-form-item label="Activity name">
            <el-input v-model="form.title"></el-input>
          </el-form-item>
          <el-form-item label="Activity form">
            <el-input type="textarea" v-model="form.body"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">Create</el-button>
            <el-button>Cancel</el-button>
          </el-form-item>
        </el-form>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Post",
  mounted() {
    this.loadData();
  },
  data() {
    return {
      loading: false,
      tableData: [],
      form: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    onSubmit() {
      axios
        .post("http://les.loc/api/posts", this.form)
        .then((res) => {
          console.log(res);
        })
        .finally(() => {
          this.loadData();
        })
        .catch((err) => {
          console.log(err);
        });
    },
    loadData() {
      this.loading = true;
      axios
        .get("http://les.loc/api/posts")
        .then((res) => {
          this.tableData = res.data.data.posts;
        })
        .finally(() => {
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
