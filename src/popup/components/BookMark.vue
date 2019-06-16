<style>
.bookshelf {
  width: 100%;
}
.bookmarks {
  overflow: hidden;
  width: 100%;
  height: auto;
  list-style: none;
  padding-left: 20px;
  transition: height 1s ease;
}
.bookmarks.unfold {
  height: auto;
}
.bookmarks.fold {
  height: 0;
}
.book, .folder {
  display: flex;
  padding: 8px 8px 8px 16px;
  cursor: pointer;
  text-decoration: none;
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
}
.book:hover, .folder:hover {
  background: -webkit-linear-gradient(top, #ffffff 1%,#eaeaea 100%);;
}
.icon {
  width: 16px;
  height: 16px;
}
.title {
  margin-left: 8px;
}

</style>

<template>
<div class="bookshelf">
  <div class="folder" @click="toggle">
    <img class="icon" :src="getFavicon(bookmarks)" alt="bookmarks.title" />
    <span class="title">{{bookmarks.title}}</span>
  </div>
  <ul :class="['bookmarks', isSpead ? 'unfold' : 'fold']">
    <li  v-for="book of bookmarks.children" :key="book.id">
      <book-marks 
        v-if="hasChildren(book)" 
        :bookmarks="book"
        :is-toggle="false"
      >
      </book-marks>
      <div v-else class="book" @click="openBook(book)">
        <img class="icon" :src="getFavicon(book)" alt="book.title" />
        <span class="title">{{book.title}}</span>
      </div>
    </li>
  </ul>
</div>

</template>

<script>
export default {
  name: 'BookMarks',
  props: {
    bookmarks: {
      type: Object,
      default: () => {}
    },
    isToggle: false
  },
  data() {
    return {
      isSpead: false
    }
  },
  created() {
    if (this.isToggle) {
      this.isSpead = true
    }
  },
  methods: {
    getFavicon(book) {
      if (book.children) {
        return 'icons/folder.png'
      } else if (book.url) {
        return `chrome://favicon/${book.url}`
      }
      return 'icons/favorite.png'
    },
    hasChildren(book) {
      return Array.isArray(book.children)
    },
    toggle() {
      this.isSpead = !this.isSpead
    },
    openBook(book) {
      const { url } = book
      chrome.tabs.create({ url, active: true }, () => window.close())
    }
  }
}
</script>