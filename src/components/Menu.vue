<template>
  <div class="Menu">
    <div
      class="Item"
      :class="{active: isActive(item.slug)}"
      :key="item.slug"
      v-for="item in menu">
      <a :href="`#${item.slug}`" @click="jumpTo(item.slug)">{{ item.title }}</a>
    </div>
  </div>
</template>

<script>
import jump from 'jump.js'

export default {
  props: ['menu'],

  data() {
    return {
      hash: ''
    }
  },

  methods: {
    isActive(slug) {
      return slug === this.hash.slice(1)
    },

    handleHashChange() {
      this.hash = location.hash
    },

    jumpTo(id) {
      jump(`#${id}`)
    }
  },

  mounted() {
    window.addEventListener('hashchange', this.handleHashChange)
  },

  beforeDestroy() {
    window.removeEventListener('hashchange', this.handleHashChange)
  }
}
</script>

<style scoped>
.Menu {
  position: sticky;
  top: 50px;
  color: var(--fg-dark);
}

.Item {
  padding-bottom: 15px;
}

.Item > a {
  position: relative;
  user-select: none;
  transition: color 200ms;
  color: var(--fg);
}

.Item > a:before {
  content: "";
  position: absolute;
  width: 100%;
  height: 1px;
  bottom: -5px;
  left: 0;
  background-color: var(--fg-dark);
  visibility: hidden;
  transform: scaleX(0);
  transform-origin: left center;
  transition: all 250ms var(--ease);
}

.Item > a:hover,
.Item.active > a {
  color: var(--fg-dark);
}

.Item > a:hover:before {
  visibility: visible;
  transform: scaleX(1);
}
</style>
