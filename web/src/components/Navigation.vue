<template>
  <nav>
    <div class="nav">
      <div class="hamburger" @click="toggleMenu()">
        <span class="screen-reader-text">Main Menu</span>
        <div :class="[ showMenu ? 'hamburger__icon hamburger__icon--active' : 'hamburger__icon' ]"></div>
      </div>
    </div>
    <div :class="[ showMenu ? 'nav__active' : 'nav__inactive' ]">
      <ul v-for="edge in $static.allSanityPage.edges" :key="edge.node.id">
        <li v-if="$context.locale === 'en-gb'">
          <g-link  :to="`/en/${edge.node.slug.current}/`">
            {{ edge.node.title.en }}
          </g-link>
        </li>
        <li v-else>
          <g-link :to="`/pl/${edge.node.slug.current}/`">
            {{ edge.node.title.pl }}
          </g-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<static-query>
  query {
    allSanityPage(sortBy: "_createdAt", order: ASC) {
      edges {
        node {
          id
          title {
            en
            pl
          }
          slug {
            current
          }
        }
      }
    }
  }
</static-query>

<script lang="ts">

export default {
  name: 'FrontNav',
  data() {
    return {
      showMenu: false,
    }
  },
  methods: {
    toggleMenu() {
      this.showMenu = !this.showMenu
    }
  }
}

</script>

<style lang="scss" scoped>
@use '../assets/scss/colors' as c;
@use '../assets/scss/breakpoints' as b;

::selection {
  background: rgba(c.$grey-100, 0.3);
}

.nav {
  z-index: 3;
  @include b.mq(lg) {
    font-size: 10px;
    top: 0;
    right: 0;
    padding: 0;
  }
  &__active,
  &__inactive {
    padding: 5em 3.3em 0 0;
    z-index: -2;
    background: rgba(c.$grey-000, 0.9);
    width: 100vw;
    height: 100vh;
    position: absolute;
    top: 0;
    right: 0;
    @include b.mq(lg) {
      padding: 5em 3em 0 0;
    }
    @include b.mq(sm) {
      padding: 5.4em 1.8em 0 0;
    }
  }
  &__inactive {
    visibility: hidden;
    clip-path: polygon(0 0, 100% 0, 100% 0, 0 0);
    transition: clip-path 1s, visibility 0s;
    transition-delay: 0s, 1s;
    transition-property: clip-path, visibility;
    transition-timing-function: ease-out;
  }
  &__active {
    visibility: visible;
    clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
    transition: clip-path 1s ease-in;
  }
}

a {
  display: inline;
  color: c.$grey-850;
  margin: auto;
  opacity: 0.6;
  transition: opacity 0.3s;
  &:hover {
    opacity: 1;
    transition: opacity 0.3s;
    text-decoration: none;
  }
}

.active--exact {
  color: c.$grey-850;
  opacity: 1;
  &:hover {
    opacity: 1;
    cursor: default;
  }
}

ul {
  display: inline;
  list-style: none;
  font-size: 6rem;
  font-weight: 300;
  line-height: 1.2;
  text-transform: uppercase;
  display: flex;
  flex-direction: column;
  justify-content: right;
  @include b.mq(lg) {
    font-size: 5.5rem;
  }
  @include b.mq(sm) {
    font-size: 5rem;
  }
  li {
    margin-left: auto;
    position: relative;
  }
}

.hamburger {
  font-size: 5px;
  display: grid;
  cursor: pointer;
  height: 5.8em;
  width: 8em;
  margin: 9em 15em 3.5em auto;
  opacity: 0.6;
  transition: opacity 0.3s;
  &:hover {
    opacity: 1;
    transition: opacity 0.3s;
  }
  @include b.mq(lg) {
    font-size: 5.5px;
    margin: 7em 8.5em 2em auto;
  }
  @include b.mq(sm) {
    font-size: 5px;
    margin: 5.5em 5em 2em auto;
  }
  &__icon {
    font-size: 6px;
    position: relative;
      width: 8em;
    height: 0.4em;
    margin: 2.6em 0 3em;
    background-color: rgba(c.$grey-850, 1);
    transition: background-color 0s;
    transition-delay: 0.5s;
    &::before,
    &::after {
      position: absolute;
      content: "";
      display: block;
      background: c.$grey-850;
      width: 8em;
      height: 0.4em;
      transform: rotate(0);
      transition: transform 0.5s, top 0.5s;
      transition-delay: 0s, 0.5s;
      transition-property: transform, top;
    }
    &::before {
      top: -2.3em;
    }
    &::after {
      top: 2.3em;
    }
    &--active {
      background-color: rgba(c.$sepia-150, 0);
      transition: background-color 0s;
      transition-delay: 0.5s;
      &::before,
      &::after {
        top: 0;
        transition: top 0.5s, transform 0.5s;
        transition-delay: 0s, 0.5s;
        transition-property: top, transform;
      }
      &::before {
        transform: rotate(0.125turn);
      }
      &::after {
        transform: rotate(-0.125turn);
      }
    }
    @include b.mq(lg) {
      font-size: 5.5px;
    }
    @include b.mq(sm) {
      font-size: 5px;
    }
  }
}

</style>
