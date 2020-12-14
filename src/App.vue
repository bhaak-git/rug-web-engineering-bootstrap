<template>
  <div class="container overflow-hidden">
    <div class="row g-5 mt-1">
      <div class="col-md">
        <h3>Create a new user</h3>
        <form @submit.prevent="submitUser" v-if="!loading.form">
          <div class="mb-3">
            <label for="first_name" class="form-label">First name</label>
            <input type="text" class="form-control" placeholder="John" id="first_name" v-model="form.first_name" />
          </div>
          <div class="mb-3">
            <label for="last_name" class="form-label">Last name</label>
            <input type="text" class="form-control" placeholder="Smith" id="last_name" v-model="form.last_name" />
          </div>
          <div class="mb-3">
            <label for="email" class="form-label">Email address</label>
            <input type="email" class="form-control" placeholder="john.smith@example.com" id="email" v-model="form.email" />
          </div>
          <button type="submit" class="btn btn-primary">Create user</button>
        </form>
        <p v-else>Loading...</p>
      </div>

      <div class="col-md">
        <div class="mb-3" v-if="!loading.list">
          <h3>Select a user</h3>
          <select class="form-select" v-if="users.length > 0">
            <option selected disabled>-</option>
            <option
              v-for="user in users"
              :key="user.id"
              @click="highlightUser(user.id)"
            >{{ user.first_name }} {{ user.last_name }}</option>
          </select>
        </div>
        <p v-else>Loading...</p>

        <span v-if="highlightedUser == null && !loading.highlight">No user selected</span>

        <div class="card" v-else-if="!loading.highlight">
          <div v-for="(field, key) in highlightedUser" :key="field">
            <img :src="field" class="card-img-top" alt="avatar" v-if="key == 'avatar'" />
          </div>
          <ul class="list-group list-group-flush">
            <div v-for="(field, key) in highlightedUser" :key="field">
              <li class="list-group-item" v-if="key !== 'avatar'">
                <strong>{{ key }}:</strong>
                {{ field }}
              </li>
            </div>
          </ul>
        </div>
        <p v-else>Loading...</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      form: null,
      loading: {
        list: false,
        form: false,
        highlight: false,
      },
      highlightedUser: null,
    };
  },

  mounted() {
    this.resetForm();
    this.fetchUsers();
  },

  methods: {
    async fetchUsers() {
      this.loading.list = true;
      let { data } = await this.$http.get("users?per_page=100&delay=1");

      this.users = data.data;
      this.loading.list = false;
    },
    async submitUser() {
      this.loading.form = true;
      let { data } = await this.$http.post("users?delay=1", this.form);
      this.resetForm();
      this.users.push(data);
      this.loading.form = false;
    },
    async highlightUser(userId) {
      this.loading.highlight = true;
      let { data } = await this.$http.get("users/" + userId + "?delay=1");
      this.highlightedUser = data.data;
      this.loading.highlight = false;
    },
    resetForm() {
      this.form = {
        first_name: null,
        last_name: null,
        email: null,
      };
    },
  },
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.css";

body {
  background-color: #f5f5f5;
}
</style>
