
<template>
  <div>
    <table class="table tablesorter text-left border border-zinc-500 bg-zinc-800z w-full">
      <thead>
        <tr class="bg-zinc-800">
          <slot name="columns">
            <th v-for="column in columns" :key="column" class="px-2.5 py-0.5">{{ column }}</th>
          </slot>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) of usersList" :key="index" class="odd:bg-zinc-700">
          <slot :row="user">
            <td v-for="(column, index) in columns" class="px-2.5 py-1">
              <div class="flex flex-row items-center" v-if="column === 'user'"><img class="w-10 rounded-full mr-3" :src="user.image" /> {{ user.firstname }} {{ user.lastname }}</div>
              <div v-if="column === 'email'">{{ user.email }}</div>
              <div v-if="column === 'username'">{{ user.username }}</div>
              <div v-if="column === 'created at'">{{ new Date(user.date).toLocaleDateString('es') }}</div>
              <div v-if="column === 'status'">{{ user.status ? 'Active' : 'Inactive' }}</div>
            </td>
          </slot>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ref, watch, defineComponent } from 'vue'
export default defineComponent({
  props: {
    user: {
      type: String,
      required: true
    }
  },
  async created() {
    const response = await fetch("https://run.mocky.io/v3/c2c1f79c-3403-4406-be1b-cf9220d23c28");
    const data = await response.json();
    data.data.forEach(user => {
      const foundUser = data.data.find(user => user.id === +this.user)
      if (foundUser.user === user.user) {
        this.usersList = user.data
      }
    })
  },
  data() {
    return {
      usersList: {},
      columns: ['user', 'username', 'email', 'created at', 'status']
    }
  }
})
</script>

<style scoped>
</style>
