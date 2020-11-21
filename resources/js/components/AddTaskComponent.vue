<template>
  <div>
    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-primary"
      data-toggle="modal"
      data-target="#exampleModal"
    >
      Create task
    </button>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Create Task</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="name">Task name</label>
                <textarea
                  name="name"
                  id="name"
                  cols="30"
                  rows="4"
                  class="form-control"
                  v-model="name"
                ></textarea>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              Close
            </button>
            <button type="button" class="btn btn-primary" @click="taskStore">
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "",
    };
  },
  methods: {
    taskStore() {
      axios
        .post("http://localhost:8000/tasksList", {
          name: this.name,
        })
        .then((response) => {
          this.$emit("task-added", response);
          this.name = "";
          $("#exampleModal").modal("hide");
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>