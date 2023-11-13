
<template>
  <div>
    <div v-for="(post, index) of posts" class="flex flex-col justify-start w-full gap-4 p-4 bg-white rounded-xl dark:bg-zinc-700/60 md:flex-row mb-4 items-center">
      <div class="relative">
        <img :src="post.thumbnailUrl" class="rounded-xl w-72 max-w-none md:w-autoz md:max-h-40" />
      </div>
      <div class="flex flex-col justify-between items-center">
        <div class="flex flex-col justify-center my-2 text-gray-700 dark:text-white md:m-0">
          <p class="text-sm text-left">
            <span class="mr-8">{{ post.account }}</span> <span class="bg-green-500/80 px-2 py-0.5 rounded-full">{{ new Date(post.date).toLocaleString() }}</span>
          </p>
          <p class="text-2xl text-left"> {{ post.title }} </p>
          <p class="text-sm text-left font-normal mb-2"> {{ post.body }} </p>
          <div class="flex flex-row flex-wrap">
            <p v-for="hashtag of post.hashtags" class="text-xs text-left font-normal bg-red-500/90 mr-1 px-2 py-[1px] rounded-full "> #{{ hashtag }} </p>
          </div>
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
    const usersResponse = await fetch("https://run.mocky.io/v3/c3db68b1-47e6-45e5-a88a-e4c482c4de06");
    const usersData = await usersResponse.json();

    const postsResponse = await fetch("https://run.mocky.io/v3/04ecea1d-ea0e-46c8-ba39-fabd53897f8d");
    const postsData = await postsResponse.json();

    const posts = []
    postsData.data.forEach(post => {
      const foundUser = usersData.data.find(user => user.id === +this.user)
      if (foundUser.user === post.user) {
        posts.push(post)
      }
    })
    this.posts = posts

    this.hashtags()
  },
  data() {
    return {
      posts: {},
      usedCreditsPercentage: `w-[${Math.round(this.credits?.usedCredits / this.credits?.totalCredits) * 100}%]`,
      hashtags: function () {
        const hashtags = this.posts.map(post => {
          const _hashtags = post.title.split(' ');

          return post.hashtags = _hashtags
        })
        return hashtags
      }
    }
  },
})
</script>
