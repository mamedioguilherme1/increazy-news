<template>
  <ul v-if="posts.length > 0 && postType == 'blog'" class="cards inc-blogs">
    <li v-for="(post, index) in posts" :key="index">
      <nuxt-link :to="`${postType}/${post.slug}`" class="card card--clickable">
        <template>
          <span class="w-full">
            <span class="flex justify-between align-baseline">
              <h3 class="card-title">{{ post.title }}</h3>
              <h6
                v-if="post.createdAt"
                class="self-start inline-block mt-0 py-1 px-2 bg-gray text-white text-base font-medium rounded-sm whitespace-no-wrap"
              >
                {{ formatDate(post.createdAt) }}
              </h6>
            </span>
            <p class="mt-2">{{ post.description }}</p>
          </span>
        </template>
      </nuxt-link>
    </li>
  </ul>

  <div v-else-if="posts.length > 0 && postType == 'docs'" class="inc-docs">
    <div v-for="tag in tags" :key="tag">
      {{ tag.label }}

      <ul class="cards">
        <li v-for="(post, index) in posts" :key="index" class="card-li" v-if="tag.value == post.tags">
          <nuxt-link :to="`${postType}/${post.slug}`" class="card card--clickable">
            <template>
              <span class="w-full">
                <span class="flex justify-between align-baseline">
                  <h3 class="card-title">{{ post.title }}</h3>
                  <h6
                    v-if="post.createdAt"
                    class="self-start inline-block mt-0 py-1 px-2 bg-gray text-white text-base font-medium rounded-sm whitespace-no-wrap"
                  >
                    {{ formatDate(post.createdAt) }}
                  </h6>
                  <div class="inc-docs__tag">
                    <small> #{{ post.tags }} </small>
                  </div>
                </span>
                <p class="mt-2">{{ post.description }}</p>
              </span>
            </template>
          </nuxt-link>
        </li>
      </ul>
    </div>
  </div>

  <div v-else-if="loading" class="cards">
    <div v-for="placeholder in placeholderClasses" :key="placeholder.id" class="card">
      <content-placeholders :rounded="true" :class="placeholder">
        <content-placeholders-heading />
      </content-placeholders>
    </div>
  </div>

  <p v-else class="max-w-5xl mx-auto">
    {{ amount > 1 ? 'Posts not found' : 'Post not found' }}
  </p>
</template>

<script>
export default {
  name: 'Posts',
  props: {
    postType: {
      type: String,
      default: 'blog',
      validator: (val) => ['blog', 'projects', 'docs'].includes(val),
    },
    amount: {
      // ? https://content.nuxtjs.org/fetching#limitn
      type: Number,
      default: 10,
      validator: (val) => val >= 0 && val < 100,
    },
    sortBy: {
      // ? https://content.nuxtjs.org/fetching#sortbykey-direction
      type: Object,
      default: () => ({
        key: 'slug',
        direction: 'desc', // you probably want 'asc' here
      }),
      validator: (obj) => typeof obj.key === 'string' && typeof obj.direction === 'string',
    },
  },
  data() {
    return {
      posts: [],
      loading: true,
      tags: [
        { label: 'Hooks', value: 'hooks' },
        { label: 'Páginas', value: 'paginas' },
        { label: 'Componentes', value: 'componentes' },
        { label: 'Loading Skeleton', value: 'skeleton' },
      ],
    }
  },
  computed: {
    placeholderClasses() {
      const classes = ['w-full', 'w-2/3', 'w-5/6']
      return [...Array.from({ length: this.amount }, (v, i) => classes[i % classes.length])] // repeats classes after one another
    },
  },
  async mounted() {
    this.loading = true
    this.posts = await this.fetchPosts()
    this.loading = false
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    async fetchPosts(postType = this.postType, amount = this.amount, sortBy = this.sortBy) {
      return this.$content(postType)
        .sortBy(sortBy.key, sortBy.direction)
        .limit(amount)
        .fetch()
        .catch((err) => {
          error({ statusCode: 404, message: amount > 1 ? 'Posts not found' : 'Post not found' })
        })
    },
  },
}
</script>

<style lang="postcss" scoped>
.card-li {
  position: relative;
}
.inc-docs__tag {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 15px 15px;
}
.inc-docs__tag small {
  padding: 3px 5px;
  background: #000;
  color: #fff;
  border-radius: 0.125rem;
}
.inc-docs__tag small:not(:last-child) {
  margin-right: 3px;
}
</style>