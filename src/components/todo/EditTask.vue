<template>
  <div class="content">
    <div class="chip">
      <img src="../../assets/logo.png" alt="Person" />
      Mtcn
    </div>

    <div id="custom-flex" class="container">
      <user-section/>
      <div class="data-card">
        <div class="card-body">
          <div v-if="currentTask" class="edit-form">
            <h4>Task</h4>
            <form>
              <div class="form-group">
                <label for="title">Title</label>
                <input
                  type="text"
                  class="form-control"
                  id="title"
                  v-model="currentTask.title"
                />
              </div>
              <div class="form-group">
                <label for="description">Description</label>
                <input
                  type="text"
                  class="form-control"
                  id="description"
                  v-model="currentTask.description"
                />
              </div>

              <div class="form-group d-flex justify-content-around">
                <div class="form-check form-switch checkbox-xl">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    id="flexSwitchCheckDefault"
                    v-model="currentTask.completed"
                  />

                  <label
                    class="form-check-label ml-4"
                    for="flexSwitchCheckDefault"
                    >{{
                      currentTask.completed ? " Completed" : " Not Completed"
                    }}</label
                  >
                </div>
              </div>
            </form>

            <div class="d-flex justify-content-around">
              <button class="btn btn-danger m-2" @click="deleteTask">
                Delete
              </button>

              <button
                type="submit"
                class="btn btn-success m-2"
                @click="updateTask"
              >
                Update
              </button>
            </div>

            <div class="text-center">
              <div v-if="success">
                <h4 class="text-success">{{ message }}</h4>
                <button
                  v-if="success"
                  class="btn btn-primary"
                  @click="showTasks"
                >
                  Todo List
                </button>
              </div>

              <h4 v-else class="text-danger">{{ message }}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "task",
  data() {
    return {
      success: false,
      currentTask: null,
      message: "",
    };
  },
  methods: {
    getTask(id) {
      this.$store
        .dispatch("task/get", id)
        .then((response) => {
          this.currentTask = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    updateCompleted() {
      this.currentTask.completed = !this.currentTask.completed;
    },

    updateTask() {
      this.$store
        .dispatch("task/update", this.currentTask)
        .then((response) => {
          console.log(response.data);
          this.success = true;
          this.message = "The task was updated successfully!";
        })
        .catch((e) => {
          this.success = false;
          this.message = "An error occurred please try again later";
          console.log(e);
        });
    },

    deleteTask() {
      this.$store
        .dispatch("task/delete", this.currentTask.id)
        .then((response) => {
          console.log(response.data);
          this.$router.push({ name: "tasks" });
        })
        .catch((e) => {
          console.log(e);
        });
    },
    showTasks() {
      this.$router.push({ name: "tasks" });
    },
  },
  components: {
    "user-section": require("../User.vue").default,
  },
  mounted() {
    this.message = "";
    this.getTask(this.$route.params.id);
  },
};
</script>
