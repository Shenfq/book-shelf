<style>
body {
  margin: 0;
  overflow-x: hidden;
  overflow-y: scroll;
  width: 300px;
  height: 600px;
}
</style>

<template>
  <div class="app">
    <book-marks 
      v-for="child of bookmarks"
      :key="child.id"
      :bookmarks="child"
      :is-toggle="true"
    >
    </book-marks >
  </div>
</template>

<script>
import BookMarks from './BookMark'
export default {
  name: 'app',
  components: {
    BookMarks,
  },
  data() {
    return {
      bookmarks: []
    }
  },
  created() {
    // eslint-disable-next-line
    chrome.bookmarks.getTree(this.initMenu);
  },
  methods: {
    initMenu([nodes]) {
      const bookmarks = []
      const children = nodes.children
      for (let child of children) {
        bookmarks.push(child)
      }
      this.bookmarks = bookmarks
    },
    getFavicon(book) {
      if (book.children) {
        return 'icons/folder.png'
      } else if (book.url) {
        return `chrome://favicon/${book.url}`
      }
      return 'icons/favorite.png'
    }
  }
}
</script>