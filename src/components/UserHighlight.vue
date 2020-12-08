<template>
    <div>
        <h3>Highlighted user</h3>
        <span v-if="user == null && !loading">No highlighted user</span>
        <div v-else-if="!loading">
            <div v-for="(field, key) in user" :key="field">
                <span v-if="key !== 'avatar'">
                    <strong>{{ key }}:</strong> {{ field }}
                </span>
                <span v-else>
                    <img :src="field">
                </span>
            </div>
        </div>
        <p v-else>Loading...</p>
    </div>
</template>

<script>
  export default {
    props: {
      user: {
        required: true
      },
      loading: {
        required: true,
        type: Boolean
      }
    }
  }
</script>

<style scoped>

</style>
