<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Search by title"
          v-model="title"/>
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button"
            @click="searchTitle"
          >
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h4>Task List</h4>
      <ul class="list-group">
        <li class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(task, index) in tasks"
          :key="index"
          @click="setActiveTask(task, index)"
        >
          {{ task.title }}
        </li>
      </ul>
      <button class="m-3 btn btn-sm btn-danger" @click="removeAllTasks">
        Remove All
      </button>
    </div>
    <div class="col-md-6">
      <div v-if="currentTask">
        <h4>Task</h4>
        <div>
          <label><strong>Title:</strong></label> {{ currentTask.title }}
        </div>
        <div>
          <label><strong>Description:</strong></label> {{ currentTask.description }}
        </div>
        <div>
          <label><strong>Status:</strong></label> {{ currentTask.published ? "Published" : "Pending" }}
        </div>
        <router-link :to="'/tasks/' + currentTask.id" class="badge badge-warning">Edit</router-link>
      </div>
      <div v-else>
        <br />
        <p>Please click on a Task...</p>
      </div>
    </div>
  </div>
</template>

<script>
import TaskDataService from "../services/TaskDataServices";
export default {
  name: "tasks-list",
  data() {
    return {
      tasks: [],
      currentTask: null,
      currentIndex: -1,
      title: ""
    };
  },
  methods: {
    retrieveTasks() {
      TaskDataService.getAll()
        .then(response => {
          this.tasks = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    refreshList() {
      this.retrieveTasks();
      this.currentTask = null;
      this.currentIndex = -1;
    },
    setActiveTask(task, index) {
      this.currentTask = task;
      this.currentIndex = task ? index : -1;
    },
    removeAllTasks() {
      TaskDataService.deleteAll()
        .then(response => {
          console.log(response.data);
          this.refreshList();
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    searchTitle() {
      TaskDataService.findByTitle(this.title)
        .then(response => {
          this.tasks = response.data;
          this.setActiveTask(null);
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.retrieveTasks();
  }
};
</script>

<style>

.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}

</style>