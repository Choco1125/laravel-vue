<template>
  <div class="container">
    <div class="row mt-2 mb-2">
      <add-task @task-added="refresh"></add-task>
    </div>
    <div class="row justify-content-center">
      <ul class="list-group col-12">
        <li class="list-group-item" v-for="task in tasks.data" :key="task.id">
          <a href="#">{{ task.name }}</a>
          <button
            class="btn btn-primary"
            data-toggle="modal"
            data-target="#editModal"
            @click="getTask(task.id)"
          >
            Edit
          </button>
          <button class="btn btn-danger" @click="deleteTask(task.id)">
            Delete
          </button>
        </li>
        <edit-task
          v-bind:taskToEdit="taskToEdit"
          @task-updated="refresh"
        ></edit-task>
      </ul>
    </div>
    <div class="row">
      <pagination
        :data="tasks"
        @pagination-change-page="getResults"
        class="mt-5"
      ></pagination>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: {},
      taskToEdit: "",
    };
  },
  created() {
    axios
      .get("http://localhost:8000/tasksList")
      .then((response) => {
        this.tasks = response.data;
        console.log(response.data);
      })
      .catch((error) => console.log(error));
  },
  methods: {
    getResults(page = 1) {
      axios
        .get("http://localhost:8000/tasksList?page=" + page)
        .then((response) => (this.tasks = response.data));
    },
    getTask(id) {
      axios
        .get("http://localhost:8000/tasks/edit/" + id)
        .then((response) => (this.taskToEdit = response.data))
        .catch((error) => console.log(error));
    },
    deleteTask(id) {
      axios
        .delete("http://localhost:8000/tasks/" + id)
        .then((response) => (this.tasks = response.data))
        .catch((error) => console.log(error));
    },
    refresh(tasks) {
      this.tasks = tasks.data;
    },
  },
  mounted() {
    console.log("Component mounted.");
  },
};
</script>
