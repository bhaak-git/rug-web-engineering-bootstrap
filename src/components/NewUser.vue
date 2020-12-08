<template>
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
</template>

<script>
  export default {
    data() {
      return {
        form: {},
        loading: false
      }
    },
    mounted() {
      this.resetForm();
    },
    methods: {
      async submitUser() {
        this.loading = true;
        let {data} = await this.$http.post('users?delay=1', this.form);
        this.resetForm();
        this.$emit('added', data);
        this.loading = false;
      },
      resetForm() {
        this.form = {
          first_name: null,
          last_name: null,
          email: null,
        };
      }
    }
  }
</script>

<style scoped>

</style>
