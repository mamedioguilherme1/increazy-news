<template>
  <nav class="scrim-bg fixed z-40 top-0 inset-x-0 pt-3 px-3 inc-header" aria-label="Main Menu">
    <div class="inc-header__menu">
      <div class="inc-header__menu-hamburger-icon" @click="toggleMenu()">
        <span></span>
        <span class="second"></span>
        <span></span>
      </div>

      <div class="inc-header__menu-ul-back" @click="toggleMenu()"></div>
      <ul class="inc-header__menu-ul">
        <li class="flex inc-header__menu-li" @click="toggleMenu()">
          <nuxt-link class="btn block" to="/docs">Documentação</nuxt-link>
        </li>
        <li class="flex inc-header__menu-li" @click="toggleMenu()">
          <nuxt-link class="btn block" to="/blog">Blog</nuxt-link>
        </li>
      </ul>
    </div>

    <div class="inc-header__search">
      <div class="inc-header__search-form">
        <input type="text" v-model="searchTerm" placeholder="o que está buscando?" />
        <img src="~/assets/imgs/icons/icon-search.svg" alt="Search" />
      </div>

      <div class="inc-header__search-results" v-show="searchTerm">
        <ul class="inc-header__search-results-ul">
          <li class="inc-header__search-results-li" v-for="i in 10" :key="i"><a href="#">Resultado 1</a></li>
        </ul>
      </div>
    </div>

    <nuxt-link class="inc-header__logo" to="/">
      <img src="~/assets/imgs/logo.png" alt="Increazy" class="isdesktop" />
      <img src="~/assets/imgs/logo-icon.png" alt="Increazy" class="ismobile" />
    </nuxt-link>
  </nav>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      searchTerm: '',
    }
  },
  methods: {
    toggleMenu() {
      const menu = document.querySelector('.inc-header__menu-ul')
      const back = document.querySelector('.inc-header__menu-ul-back')
      const bread = document.querySelector('.inc-header__menu-hamburger-icon')

      menu.classList.toggle('inc-header__menu-ul--active')
      back.classList.toggle('inc-header__menu-ul-back--active')
      bread.classList.toggle('inc-header__menu-hamburger-icon--active')
    },
  },
}
</script>
 
<style lang="postcss" scoped>
.inc-header {
  width: 100%;
  display: flex;
  flex-flow: row;
  justify-content: center;
  align-items: center;
}
.inc-header__menu {
  display: flex;
  position: absolute;
  top: 12px;
  left: 12px;
}
.inc-header__menu-hamburger-icon {
  display: flex;
  width: 40px;
  height: 38px;
  flex-flow: column;
  justify-content: space-between;
  align-items: flex-start;
  cursor: pointer;
  z-index: 100;
  padding: 5px;
  border: 1px solid var(--border-color);
  border-radius: 0.25rem;
}
.inc-header__menu-hamburger-icon span {
  width: 100%;
  height: 3px;
  background: var(--color-primary-800);
  border-radius: 20px;
  transition: all 0.3s;
}
.inc-header__menu-hamburger-icon span:nth-child(2) {
  width: 80%;
  transition: all 0.3s;
}
.inc-header__menu-hamburger-icon:hover span:nth-child(2) {
  width: 100% !important;
  transition: all 0.3s;
}
.inc-header__menu-hamburger-icon--active span:nth-child(2) {
  width: 100% !important;
  transition: all 0.3s;
}

.inc-header__menu-ul-back {
  position: fixed;
  width: 0;
  height: 0;
  z-index: -98;
}
.inc-header__menu-ul-back--active {
  height: 100% !important;
  width: 100% !important;
  z-index: 98 !important;
}

.inc-header__menu-ul {
  width: 235px;
  background: #fff;
  padding: 0;
  border-radius: 0.25rem;
  box-shadow: 3px 3px 15px 0 rgb(0 0 0 / 20%);
  position: absolute;
  top: 43px;
  left: 0;
  transition: all 0.3s;
  overflow: hidden;
  height: 0;
  z-index: -99;
}
.inc-header__menu-ul--active {
  padding: 10px !important;
  height: auto !important;
  z-index: 99 !important;
  transition: all 0.3s;
}

.inc-header__menu-li .btn {
  width: 100%;
  background: unset !important;
  border: 0;
  transition: all 0.3s;
  cursor: pointer;
}
.inc-header__menu-li .btn:hover {
  transition: all 0.3s;
  background-color: var(--color-primary-800) !important;
  color: #fff;
}

.inc-header__search {
  position: absolute;
  top: 12px;
  left: 105px;
}
.inc-header__search-form {
  width: 100%;
  display: flex;
  flex-flow: row;
  justify-content: flex-start;
  align-items: center;
  position: relative;
}
.inc-header__search-form input {
  background: transparent;
  padding: 7px 30px 7px 10px;
  height: 38px;
}
.inc-header__search-form img {
  width: 20px;
  position: absolute;
  top: 9px;
  right: 9px;
}

.inc-header__search-results {
  position: absolute;
  top: 40px;
  width: 100%;
  background: #fff;
  border-radius: 0.25rem;
  box-shadow: 0 0 15px -2px rgb(0 0 0 / 20%);
  padding: 10px;
}
.inc-header__search-results-ul {
  display: flex;
  flex-flow: column;
  justify-content: flex-start;
  align-items: flex-start;
  max-height: 300px;
  overflow-y: auto;
}
.inc-header__search-results-li {
  font-size: 14px;
  margin: 5px 0;
}
.inc-header__search-results-li a:hover {
  text-decoration: underline;
}

.inc-header__logo img {
  width: 180px;
}
.inc-header__logo {
  background: unset !important;
  cursor: pointer;
}

@media (max-width: 1024px) {
  .inc-header {
    justify-content: space-between;
  }
  .inc-header__menu {
    position: relative !important;
    top: 0;
    left: 0;
  }
  .inc-header__search {
    position: relative !important;
    width: 60%;
    top: 0;
    left: 0;
  }
  .inc-header__search-form input {
    width: 100%;
  }
  .inc-header__logo img {
    width: auto;
    height: 40px;
  }
}

.scrim-bg {
  &::before {
    content: '';
    z-index: -1;
    background-color: var(--bg);
    @apply absolute bottom-0 inset-x-0 h-12 mb-4 transition-colors duration-200 ease-in-out;
  }
  &::after {
    content: '';
    z-index: -1;
    opacity: 1;
    animation: fadeIn1 500ms ease-in-out;
    @apply pointer-events-none absolute bottom-0 inset-x-0 h-16 -mb-12;
    background: linear-gradient(to bottom, #111827, cubic-bezier(0.15, 0, 0.45, 1), transparent);
  }
}
.nuxt-link-exact-active {
  @apply text-gray-200 border-gray-400 bg-gray-800 bg-opacity-25 cursor-default;
}

.light-mode {
  & .scrim-bg {
    &::after {
      animation-name: fadeIn2;
      background: linear-gradient(to bottom, #e5e7eb, cubic-bezier(0.15, 0, 0.45, 1), transparent);
    }
  }
  & .nuxt-link-exact-active {
    @apply text-primary-700 border-gray-600 bg-gray-100;
  }
}

/* Need two because of smoother switching between color modes */
@keyframes fadeIn1 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes fadeIn2 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
