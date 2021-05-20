<template>
  <main>
    <section v-if="posts" class="w-full max-w-5xl mx-auto">
      <h1 class="title">Documentação</h1>
      <posts post-type="docs" :amount="10" />
    </section>
  </main>
</template>

<script>
export default {
  async asyncData({ $content, error }) {
    let posts
    try {
      posts = await $content('docs').fetch()
    } catch (e) {
      error({ message: 'Doc not found' })
    }
    return { posts }
  },
}
</script>

<style lang="postcss" scoped>
.title {
  margin-bottom: 50px;
}
</style>
