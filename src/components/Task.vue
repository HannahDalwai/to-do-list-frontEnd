<template>
    <div v-if="currentTask" class="edit-form">
    <h4>Task</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
          v-model="currentTask.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description"
          v-model="currentTask.description"
        />
      </div>
      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentTask.published ? "Published" : "Pending" }}
      </div>
    </form>
    <button class="badge badge-primary mr-2"
      v-if="currentTask.published"
      @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="badge badge-primary mr-2"
      @click="updatePublished(true)"
    >
      Publish
    </button>
    <button class="badge badge-danger mr-2"
      @click="deleteTask"
    >
      Delete
    </button>
    <button type="submit" class="badge badge-success"
      @click="updateTask"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>
  <div v-else>
    <br />
    <p>Please click on a Task...</p>
  </div>

</template>

<script>

import TaskDataService from "../services/TaskDataServices";
export default {
  name: "task",
  data() {
    return {
      currentTask: null,
      message: ''
    };
  },
  methods: {
    getTask(id) {
      TaskDataService.get(id)
        .then(response => {
          this.currentTask = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    updatePublished(status) {
      var data = {
        id: this.currentTask.id,
        title: this.currentTask.title,
        description: this.currentTask.description,
        published: status
      };
      TaskDataService.update(this.currentTask.id, data)
        .then(response => {
          console.log(response.data);
          this.currentTask.published = status;
          this.message = 'The status was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },
    updateTask() {
      TaskDataService.update(this.currentTask.id, this.currentTask)
        .then(response => {
          console.log(response.data);
          this.message = 'The task was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },
    deleteTask() {
      TaskDataService.delete(this.currentTask.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "tasks" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getTask(this.$route.params.id);
  }
};

</script>

<style>

.edit-form {
  max-width: 300px;
  margin: auto;
}

</style>