<template>
  <div v-if="currentTutorial" class="edit-form">
    <h4>Tutorial</h4>
    <form>
      <div class="form-group">
        <label for="title">
          Title
          <input
            class="form-control"
            id="title"
            type="text"
            v-model="currentTutorial.title"
          />
        </label>
      </div>
      <div class="form-group">
        <label for="description">
          Description
          <input
            class="form-control"
            id="description"
            type="text"
            v-model="currentTutorial.description"
          />
        </label>
      </div>

      <div class="form-group">
        <label><strong>Status : </strong></label>
        {{ currentTutorial.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button
      class="badge badge-primary mr-2"
      v-if="currentTutorial.published"
      @click="updatePublished(false)"
    >
      Unpublish
    </button>
    <button
      v-else
      class="badge badge-primary mr-2"
      @click="updatePublished(true)"
    >
      Publish
    </button>
    <button class="badge badge-danger mr-2" @click="deleteTutorial">
      Delete
    </button>

    <button class="badge badge-success" @click="updateTutorial">Update</button>
    <p>{{ message }}</p>
  </div>
  <div v-else>
    <br />
    <p>Please click on a tutorial</p>
  </div>
</template>

<script>
import TutorialDataService from "../services/TutorialDataService";

export default {
  name: "tutorial",
  data() {
    return {
      currentTutorial: null,
      message: "",
    };
  },
  methods: {
    getTutorial(id) {
      TutorialDataService.get(id)
        .then((response) => {
          this.currentTutorial = response.data;
          console.log(response.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },

    updatePublished(status) {
      var data = {
        id: this.currentTutorial.id,
        title: this.currentTutorial.title,
        description: this.currentTutorial.description,
        published: status,
      };
      TutorialDataService.update(this.currentTutorial.id, data)
        .then((response) => {
          this.currentTutorial.status = status;
          console.log(response.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },

    updateTutorial() {
      TutorialDataService.update(this.currentTutorial.id, this.currentTutorial)
        .then((response) => {
          console.log(response.data);
          this.message = "Tutorial deleted successfully!";
        })
        .catch((err) => {
          console.log(err);
        });
    },

    deleteTutorial() {
      TutorialDataService.delete(this.currentTutorial.id)
        .then((response) => {
          console.log(response.data);
          this.$router.push({ name: "tutorials" });
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    this.message = "";
    this.getTutorial(this.$route.params.id);
  },
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
