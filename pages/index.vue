<template>
  <section id="posts" class="container">
    <PostPreview v-for="post in posts" :key="post.id" 
      :title="post.title"
      :excerpt="post.previewText"
      :thumbnailImage="post.thumbnailUrl"
      :id="post.id"
    />
  </section>
</template>

<script>
import PostPreview from "@/components/Blog/PostPreview";
export default {
  components:{
    PostPreview
  },

  asyncData(context){

    // let version = context.query._storyblok || context.isDev ? 'draft' : 'published'
    return context.app.$storyapi.get('cdn/stories',{
      version: 'draft',
      starts_with:"blog/"
    }).then((res) => {
      return {posts: res.data.stories.map(bp => {
        return {
          id: bp.slug,
          title: bp.content.title,
          previewText: bp.content.summary,
          thumbnailUrl: bp.content.thumbnail
        }
      })}
    }).catch((res) => {
      context.error({ statusCode: res.response.status, message: res.response.data })
    })
  }

    // data(){
    //   return {
    //     posts: [
    //     {
    //       title:'A New Beginning',
    //       previewText: "this will be awesome",
    //       thumbnailUrl:"https://images.pexels.com/photos/3689772/pexels-photo-3689772.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940",
    //       id:"a-new-beginning"
    //     },
    //     {
    //       title:'A Beginning of Avenger',
    //       previewText: "this will be awesome",
    //       thumbnailUrl:"https://images.pexels.com/photos/3689772/pexels-photo-3689772.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940",
    //       id:"a-beginning"
    //     },
    //     ]
    //   }
    // }
}
</script>

<style scoped>
.container{
  width: 90%;
  margin: auto;
}
</style>
