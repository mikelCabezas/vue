
<template>
<div>
<div v-for="(post, index) of posts" class="flex flex-col justify-start w-full gap-4 p-4 bg-white rounded-xl dark:bg-zinc-700/60 md:flex-row mb-4 items-center">
    <div class="relative">
        <img :src="post.thumbnailUrl" class="w-full rounded-xl w-72 max-w-none md:w-autoz md:max-h-40"/>
        <!-- <span class="absolute px-2 py-1 text-xs text-white rounded bg-gray-700/50 right-2 bottom-2">
            7 min
        </span> -->
    </div>
    <div class="flex flex-col justify-between items-center">
        <div class="flex flex-col justify-center my-2 text-gray-700 dark:text-white md:m-0">
            <p class="text-sm text-left">
                <span class="mr-8">{{post.account}}</span> <span class="bg-green-500/80 px-2 py-0.5 rounded-full">{{new Date(post.date).toLocaleString()}}</span>
            </p>
            <p class="text-2xl text-left">
                {{post.title}}
            </p>
            <p class="text-sm text-left font-normal mb-2">
                {{post.body}}
            </p>
            <div class="flex flex-row flex-wrap">
              <p v-for="hashtag of post.hashtags" class="text-xs text-left font-normal bg-red-500/90 mr-1 px-2 py-[1px] rounded-full ">
                  #{{hashtag}}
              </p>
            </div>
        </div>
    </div>
</div>

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
    const response = await fetch("https://run.mocky.io/v3/04ecea1d-ea0e-46c8-ba39-fabd53897f8d");
    const data = await response.json();
    this.posts = data.data;
    console.log(data)
    this.hashtags()
  },
  data() {
      return {
          posts: {},
          usedCreditsPercentage: `w-[${Math.round(this.credits?.usedCredits / this.credits?.totalCredits) * 100}%]`,
          hashtags: function() {
            const hashtags = this.posts.map(post => {
              const _hashtags = post.title.split(' ');
              
              return post.hashtags = _hashtags
            })
            return hashtags
          }
      }
  },
  computed: {
    // hashtags() {
    //         const hsh = this.posts.map(post => {
    //           const _hastags = post.title.split(' ');
              
    //           return post.hastags = _hastags
    //         })
    //         console.log('hsh', hsh)
    //       }
    }
})
</script>

<style scoped>
</style>
