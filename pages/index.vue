<template>
  <section class="blog-post">
    <h1 class="title">{{ header }}</h1>
    <div v-html="content"></div>
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
    const content = PrismicDom.RichText.asHtml(blog_post.data['blog_content'])

    return { blog_post, header, content }
  }
}
</script>
