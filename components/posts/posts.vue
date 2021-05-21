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
    <div v-for="tag in tags" :key="tag" class="inc-docs__tags" :data-tag="tag.value">
      <div class="inc-docs__title" @click="toggleTag(tag.value)">
        <span class="inc-docs__title-text">{{ tag.label }}</span>
        <div class="inc-docs__title-icon"><span>+</span></div>
      </div>

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
    toggleTag(value) {
      console.log(value)
      const tag = document.querySelector(`.inc-docs__tags[data-tag="${value}"]`)
      if (tag) {
        tag.classList.toggle('inc-docs__tags--active')
      }
    },
  },
}
</script>

<style lang="postcss" scoped>
.card-li {
  position: relative;
  width: calc(33% - 15px);
  margin-top: 0;
  margin-bottom: 15px;
}
.card-li:not(:nth-child(3n)) {
  margin-right: 15px;
}
.card-li h3 {
  font-weight: 600;
  font-size: 17px;
}
.card-li p {
  font-size: 15px;
}
.card-li h6 {
  font-size: 10px;
  position: absolute;
  top: 0;
  right: 0;
  background: transparent;
  color: var(--color-primary-100);
  font-weight: 600;
  background: var(--color-primary-700);
  box-shadow: 3px 3px 15px -3px rgb(0 0 0 / 30%);
  border-radius: 0.125rem;
}

.inc-docs__tags:not(:last-child) {
  margin-bottom: 40px;
}
.inc-docs .cards {
  height: 0;
  overflow: hidden;
  padding: 0;
  transition: all 0.4s;
  display: flex;
  flex-flow: row wrap;
  justify-content: flex-start;
  align-items: flex-start;
}
.inc-docs__tags--active {
  background: #f5f5f5;
  border-radius: 0.25rem;
}
.inc-docs__tags--active .cards {
  height: auto !important;
  padding: 15px !important;
  transition: all 0.4s;
}
.inc-docs__tags--active .inc-docs__title .inc-docs__title-icon {
  transform: rotate(135deg);
}

.inc-docs__tag {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 15px 15px;
}
.inc-docs__tag small {
  padding: 3px 5px;
  background: var(--color-gray-900);
  color: #fff;
  border-radius: 0.125rem;
}
.inc-docs__tag small:not(:last-child) {
  margin-right: 3px;
}

.inc-docs__title {
  padding-bottom: 7px;
  display: flex;
  flex-flow: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: var(--color-primary-800);
  color: #fff;
  border-radius: 0.25rem;
  box-shadow: 3px 3px 15px -2px rgb(0 0 0 / 30%);
  cursor: pointer;
}
.inc-docs__title-text {
  font-size: 17px;
}
.inc-docs__title-icon {
  font-size: 25px;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: var(--color-primary-600);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.4s;
  position: relative;
  border: 0;
}
.inc-docs__title-icon span {
  position: absolute;
  top: -5px;
}

@media (max-width: 1024px) {
  .card-li {
    width: 100%;
  }
  .card-li:not(:nth-child(3n)) {
    margin-right: unset;
  }
  .card-li:last-child {
    margin-bottom: 0;
  }
  .card-li .card {
    padding-top: 1.5rem;
    padding-bottom: 2.2rem;
  }
}
</style>