<template>
  <div id="about-section">
      <h1>{{title}}</h1>
      <p>{{ content }}</p>
  </div>
</template>

<script>
export default {
  asyncData(context){
    return context.app.$storyapi.get('cdn/stories/about', {
      version: 'draft',
    }).then(res => {
      return {
        title: res.data.story.content.title,
        content: res.data.story.content.content
      }
    })
  }
}
</script>

<style scoped>

#about-section{
  width: 80%;
  margin: auto;
}

#about-section p{
  white-space: pre-line;
}
</style>