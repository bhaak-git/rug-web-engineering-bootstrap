<template>
    <div>
        <div>
            <h1>Lets keep track of users</h1>
            <div v-if="!loading.list">
                <h3>Here is a list of all the users</h3>
                <div v-if="users.length > 0">
                    <div v-for="user in users" :key="user.id">
                        <button @click="highlightUser(user.id)">
                            {{ user.first_name }} {{ user.last_name }}
                        </button>
                    </div>
                </div>
            </div>
            <p v-else>Loading...</p>
        </div>
        <NewUser @added="user => users.push(user)"/>
        <UserHighlight :user="highlightedUser" :loading="loading.highlight"/>
    </div>
</template>

<script>
  import UserHighlight from "./components/UserHighlight";
  import NewUser from "./components/NewUser";

  export default {
    components: {
      UserHighlight,
      NewUser
    },
    data() {
      return {
        users: [],
        form: {},
        loading: {
          list: false,
          highlight: false
        },
        highlightedUser: null
      }
    },
    mounted() {
      this.fetchUsers();
    },
    methods: {
      async fetchUsers() {
        this.loading.list = true;
        let {data} = await this.$http.get('users?per_page=100&delay=1');

        this.users = data.data;
        this.loading.list = false;
      },
      async highlightUser(userId) {
        this.loading.highlight = true;
        let {data} = await this.$http.get('users/' + userId + '?delay=1');
        this.highlightedUser = data.data;
        this.loading.highlight = false;
      }
    }
  }
</script>
