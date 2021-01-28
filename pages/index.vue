<template>
  <section class="blog-post">
    <h1 class="title">{{ header }}</h1>
    <p class="paragraph" v-for="text in content">{{ text.text }}</p>
  </section>
</template>

<script>
import Prismic from 'prismic-javascript'
import PrismicDom from 'prismic-dom'
import PrismicConfig from './../prismic.config'

export default {
  async asyncData() {
    const api = await Prismic.getApi(PrismicConfig.apiEndpoint)
    let blog_post = {}

    const results = await api.query(Prismic.Predicates.at('document.type', 'blog-post'), { lang: 'cs-cz'})
    blog_post = results.results[0]

    const header = PrismicDom.RichText.asText(blog_post.data['blog_post_title'])
    const content = blog_post.data['blog_content']

    return { blog_post, header, content }
  }
}
</script>

<style scoped>
.blog-post {
  margin: 25px 0;
  padding: 0 100px;
  width: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.title {
  margin: 50px 0;
}
p {
  color: #000;
  margin: 15px 0 5px;
  max-width: 450px;
  line-height: 1.44;
  text-align: justify;
}
</style>
