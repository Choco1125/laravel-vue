<template>
  <div
    class="modal fade"
    id="editModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Edit Task</h5>
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
                v-model="taskToEdit.name"
              ></textarea>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal">
            Close
          </button>
          <button type="button" class="btn btn-primary" @click="update">
            Save changes
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["taskToEdit"],
  methods: {
    update() {
      axios
        .put("http://localhost:8000/tasks/edit/" + this.taskToEdit.id, {
          name: this.taskToEdit.name,
        })
        .then((response) => {
          this.$emit("task-updated", response);
          this.taskToEdit.name = "";
          $("#editModal").modal("hide");
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>