<template>
  <main class="inc-home">
    <section class="self-center flex flex-col flex-1 items-center justify-center inc-home__info">
      <h1 class="title text-center">Aprenda com a nossa documentação</h1>
      <nuxt-link class="btn block" to="/docs">Documentação</nuxt-link>

      <h2 class="subtitle text-center">Vejam nossas atualizações</h2>
      <nuxt-link class="btn block" to="/blog">Blog</nuxt-link>
    </section>

    <section class="mt-8">
      <h3 class="text-primary-600 dark:text-primary-400 max-w-5xl mx-auto">Novidade Recente</h3>
      <posts post-type="blog" :amount="1" />
    </section>
  </main>
</template>

<style lang="postcss" scoped>
.inc-home__info .title {
  margin-bottom: 30px;
}

.inc-home__info .subtitle {
  margin-bottom: 20px;
  margin-top: 50px;
}

.inc-home__info .btn {
  background: rgb(2, 0, 36);
  background: linear-gradient(90deg, rgba(2, 0, 36, 1) 0%, rgba(9, 27, 41, 1) 100%, rgba(18, 52, 77, 1) 100%);
  padding: 10px 40px;
  color: #fff;
  border: 0;
  box-shadow: 3px 3px 15px -2px rgba(0, 0, 0, 0.4);
  transition: all 0.2s;
}

.inc-home__info .btn:hover {
  padding: 10px 50px;
  transition: all 0.2s;
}
</style>