<template>
    <div>
        <h1>This website can retrieve the public repos of any user in github:</h1>
        <input type="text" v-model="userName">
        <button @click="fetchData">Search</button>
        <div v-if="repos.length != 0">
            <h2>Results</h2>
            <div>
                <ul>
                    <li v-for="repo in repos" :key="repo.id">
                        {{ repo.name }}
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        userName: null,
        repos: []
      }
    },
    methods: {
      async fetchData() {
        let {data} = await this.$http.get(`https://api.github.com/users/${this.userName}/repos`);

        this.repos = data;
      }
    }
  }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
