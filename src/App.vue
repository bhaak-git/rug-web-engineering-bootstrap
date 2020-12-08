<template>
    <div>
        <h1>Lets keep track of users</h1>
        <div>
            <h3>Here is a list of all the users</h3>
            <div v-if="users.length > 0">
                <div v-for="user in users" :key="user.id">
                    <button @click="highlightUser(user.id)">
                        {{ user.first_name }} {{ user.last_name }}
                    </button>
                </div>
            </div>
        </div>
        <div>
            <h3>Create a new user</h3>
            <form @submit.prevent="submitUser" v-if="!loading">
                <div>
                    <label for="first_name">
                        First name
                    </label>
                    <input type="text" id="first_name" v-model="form.first_name">
                </div>
                <div>
                    <label for="last_name">
                        Last name
                    </label>
                    <input type="text" id="last_name" v-model="form.last_name">
                </div>
                <div>
                    <label for="email">
                        Email
                    </label>
                    <input type="text" id="email" v-model="form.email">
                </div>
                <button type="submit">Create user</button>
            </form>
            <p v-else>Loading...</p>
        </div>
        <div v-if="highlightedUser != null">
            <h3>Highlighted user</h3>
            <div v-for="(field, key) in highlightedUser" :key="field">
                <span v-if="key !== 'avatar'">
                    <strong>{{ key }}:</strong> {{ field }}
                </span>
                <span v-else>
                    <img :src="field">
                </span>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        users: [],
        form: {
          first_name: null,
          last_name: null,
          email: null,
        },
        loading: false,
        highlightedUser: null
      }
    },
    mounted() {
      this.fetchUsers();
    },
    methods: {
      async fetchUsers() {
        let {data} = await this.$http.get('users?per_page=100');

        this.users = data.data;
      },
      async submitUser() {
        this.loading = true;
        let {data} = await this.$http.post('users?delay=3', this.form);
        this.loading = false;
        this.users.push(data);
      },
      async highlightUser(userId) {
        let {data} = await this.$http.get('users/' + userId);
        this.highlightedUser = data.data;
      }
    }
  }
</script>
