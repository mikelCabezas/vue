
<template>
  <div class="h-full">
    <div class="p-4 bg-white shadow-lg rounded-2xl w-full h-full  dark:bg-indigo-800 flex flex-col items-center justify-center">
      <div class="flex items-center">
        <p class="text-gray-700 text-md text-bold dark:text-gray-50 text-center w-full">Total credits</p>
      </div>
      <div class="flex flex-col justify-start w-2/3 mx-auto">
        <p class="mt-1 mb-4 text-7xl font-bold text-center text-gray-800 dark:text-white"> {{ credits.totalCredits }} </p>
        <div class="relative h-2 bg-gray-200 rounded w-full">
          <div class="absolute top-0 left-0 h-2 bg-green-500 rounded" :style="{ width: `${credits.usedCredits / this.credits.totalCredits * 100}%` }">
          </div>
        </div>
        <div class="text-center mt-3"><strong>Remaining credits:</strong>
          <p class="text-4xl font-bold text-center text-gray-800 dark:text-white"> {{ credits.availableCredits }} </p>
        </div>
        <div class="text-center mt-3"><strong>Expires in:</strong>
          <p class="text-sm text-center text-gray-800 dark:text-white">{{ new Date(credits.expires).toLocaleDateString('es') }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    user: {
      type: String,
      required: true
    },
  },
  async created() {
    const usersResponse = await fetch("https://run.mocky.io/v3/c2c1f79c-3403-4406-be1b-cf9220d23c28");
    const usersData = await usersResponse.json();

    const creditsResponse = await fetch("https://run.mocky.io/v3/d00a7719-d7f6-4959-87bd-20ccb9956e3f");
    const creditsData = await creditsResponse.json();

    creditsData.data.forEach(userCredits => {
      const foundUser = usersData.data.find(user => user.id === +this.user)
      if (foundUser.user === userCredits.user) {
        this.credits = userCredits
      }
    })

    this.usedCreditsPercentage = `w-[${(this.credits.usedCredits / this.credits.totalCredits) * 100}%]`

  },
  data() {
    return {
      credits: {},
      usedCreditsPercentage: ''
    }
  }
})
</script>