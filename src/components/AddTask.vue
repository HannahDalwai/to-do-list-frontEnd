<template>
   <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="task.title"
          name="title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input
          class="form-control"
          id="description"
          required
          v-model="task.description"
          name="description"
        />
      </div>
      <button @click="saveTask" class="btn btn-success">Submit</button>
    </div>
    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newTask">Add</button>
    </div>
  </div>
</template>

<script>

import TaskDataService from "../services/TaskDataServices";
export default {
  name: "add-task",
  data() {
    return {
      task: {
        id: null,
        title: "",
        description: "",
        published: false
      },
      submitted: false
    };
  },
  methods: {
    saveTask() {
      var data = {
        title: this.task.title,
        description: this.task.description
      };
      TaskDataService.create(data)
        .then(response => {
          this.task.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newTask() {
      this.submitted = false;
      this.task = {};
    }
  }
};

</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>