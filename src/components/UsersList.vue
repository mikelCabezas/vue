
<template>
<div>
    <h1>COMPONENT 1</h1>
    <table class="table tablesorter text-left border border-zinc-500 bg-zinc-700">
        <thead>
        <tr class="bg-zinc-700">
        <slot name="columns">
            <th v-for="column in columns" :key="column" class="px-2.5 py-0.5">{{column}}</th>
        </slot>
        </tr>
        </thead>
        <tbody>
            <tr v-for="(user, index) in users" :key="index" class="odd:bg-zinc-800">
            <slot :row="user">
                <td v-for="(column, index) in columns" class="px-2.5 py-1">
                    <div v-if="column === 'user'">{{user.firstname}} {{user.lastname}}</div>
                    <div v-if="column === 'email'">{{user.email}}</div>
                    <div v-if="column === 'username'">{{user.username}}</div>
                    <div v-if="column === 'website'">{{user.website}}</div>
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
    msg: {
      type: String,
      required: false,
    },
  },
  async created() {
  const response = await fetch("https://fakerapi.it/api/v1/users?_quantity=10");
  const data = await response.json();
  this.users = datdataa.;
  console.log(data.data)
},
data() {
    return {
        users: {},
        columns: ['user', 'username', 'email', 'website']
    }
}
})
</script>

<style scoped>
</style>
