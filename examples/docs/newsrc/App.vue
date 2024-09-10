<template>
  <div :class="themeClass">
    <header class="header">
      <h1>电子书阅读器</h1>
      <div class="header-buttons">
        <button @click="toggleTheme">{{ buttonLabel }}</button>
        <button @click="toggleFontSize">切换字体大小</button>
      </div>
    </header>
    <div class="main-container">
      <aside class="sidebar">
        <h2>书架</h2>
        <ul>
          <li v-for="book in books" :key="book.id" @click="selectBook(book)">
            {{ book.title }}
          </li>
        </ul>
        <div class="sidebar-buttons">
          <button @click="previousBook">上一本书</button>
          <button @click="nextBook">下一本书</button>
        </div>
      </aside>
      <div class="content">
        <section class="book-viewer">
          <h2>{{ currentBook.title }}</h2>
          <div class="reading-area">
            <p v-html="currentBook.content"></p>
            <div class="content-buttons">
              <button @click="increaseFontSize">增大字体</button>
              <button @click="decreaseFontSize">减小字体</button>
              <button @click="downloadChapter">下载当前章节</button>
            </div>
          </div>
        </section>
        <section class="chapters">
          <h2>章节列表</h2>
          <ul>
            <li v-for="chapter in currentBook.chapters" :key="chapter.id" @click="openChapter(chapter)">
              {{ chapter.title }}
            </li>
          </ul>
        </section>
        <section class="bookmarks">
          <h2>书签</h2>
          <ul>
            <li v-for="bookmark in bookmarks" :key="bookmark.id">
              {{ bookmark.title }}
            </li>
          </ul>
          <button @click="addBookmark">添加书签</button>
        </section>
      </div>
    </div>
    <footer class="footer">
      <p>© 2024 电子书阅读器. All rights reserved.</p>
    </footer>
  </div>
</template>

<script>
import { darkTheme } from '@opentiny/vue-theme/theme/dark-Theme/dark-theme.js'
import { galaxyTheme } from '@opentiny/vue-theme/theme/galaxy-theme/index.js'

export default {
  data() {
    return {
      isDarkTheme: false,
      isLargeFont: false,
      currentBook: {
        title: '西游记',
        content: `第1回 灵根育孕源流出 心性修持大道生...`,
        chapters: [
          { id: 1, title: '第一回 灵根育孕源流出 心性修持大道生' },
          { id: 2, title: '第二回 兔妖引来如意棒 大圣闹海重生' },
          { id: 3, title: '第三回 三藏法师大闹天宫 归国取经广育群妖' }
        ]
      },
      books: [
        { id: 1, title: '西游记' },
        { id: 2, title: '红楼梦' },
        { id: 3, title: '水浒传' }
      ],
      bookmarks: [
        { id: 1, title: '第一章开始' },
        { id: 2, title: '第二章精彩' }
      ]
    }
  },
  computed: {
    themeClass() {
      return this.isDarkTheme ? 'dark' : 'aurora'
    },
    buttonLabel() {
      return this.isDarkTheme ? '切换到 Aurora 主题' : '切换到 Dark 主题'
    }
  },
  methods: {
    toggleTheme() {
      this.isDarkTheme = !this.isDarkTheme
      this.applyTheme()
    },
    toggleFontSize() {
      this.isLargeFont = !this.isLargeFont
      this.applyFontSize()
    },
    applyTheme() {
      const themeVariables = this.isDarkTheme ? darkTheme : galaxyTheme
      for (const [key, value] of Object.entries(themeVariables)) {
        document.documentElement.style.setProperty(`--${key}`, value)
      }
    },
    applyFontSize() {
      const fontSize = this.isLargeFont ? '18px' : '14px'
      document.documentElement.style.setProperty('--reading-font-size', fontSize)
    },
    selectBook(book) {
      this.currentBook = {
        title: book.title,
        content: book.title === '西游记' ? `第1回 灵根育孕源流出...` : `这是《${book.title}》的内容部分。`,
        chapters: [
          { id: 1, title: '第一回 灵根育孕源流出 心性修持大道生' },
          { id: 2, title: '第二回 兔妖引来如意棒 大圣闹海重生' },
          { id: 3, title: '第三回 三藏法师大闹天宫 归国取经广育群妖' }
        ]
      }
    },
    openChapter(chapter) {
      this.currentBook.content = `这是《${this.currentBook.title}》的${chapter.title}内容。`
    },
    previousBook() {
      const currentIndex = this.books.findIndex((book) => book.title === this.currentBook.title)
      const previousIndex = (currentIndex - 1 + this.books.length) % this.books.length
      this.selectBook(this.books[previousIndex])
    },
    nextBook() {
      const currentIndex = this.books.findIndex((book) => book.title === this.currentBook.title)
      const nextIndex = (currentIndex + 1) % this.books.length
      this.selectBook(this.books[nextIndex])
    },
    increaseFontSize() {
      this.isLargeFont = true
      this.applyFontSize()
    },
    decreaseFontSize() {
      this.isLargeFont = false
      this.applyFontSize()
    },
    downloadChapter() {
      const blob = new Blob([this.currentBook.content], { type: 'text/plain;charset=utf-8' })
      const link = document.createElement('a')
      link.href = URL.createObjectURL(blob)
      link.download = `${this.currentBook.title}-${new Date().toISOString().slice(0, 10)}.txt`
      link.click()
    },
    addBookmark() {
      this.bookmarks.push({ id: this.bookmarks.length + 1, title: `书签 ${this.bookmarks.length + 1}` })
    }
  },
  mounted() {
    this.applyTheme()
    this.applyFontSize()
  }
}
</script>

<style>
/* Aurora 主题 */
.aurora {
  --ti-base-color-bg-1: #ffffff;
  --ti-base-color-common-7: #333333;
  --ti-button-normal-background-color: #1890ff;
  --ti-button-normal-hover-bg-color: #096dd9;
  --ti-common-color-border: #2e3243;
  --ti-common-color-light: #f5f5f5;
  --ti-common-color-info-normal: #333333;
  --ti-common-color-hover-background: #e6f7ff;
  --ti-common-color-selected-background: #1890ff;
  --ti-button-primary-text-color: #ffffff;
}

/* Dark 主题 */
.dark {
  --dark-color-brand-6: #0a84ff;
  --dark-color-common-9: #e0e0e0;
  --dark-color-common-8: #b3b3b3;
  --dark-color-bg-primary: #2b2b2b;
  --dark-color-btn-bg: #444444; /* 按钮背景色 */
  --dark-color-btn-text: #e0e0e0; /* 按钮文本颜色 */
  --dark-color-btn-hover-bg: #555555; /* 按钮悬停背景色 */
  --dark-color-btn-border: #666666; /* 按钮边框颜色 */
  --dark-color-bg-accent: #3a3a3a;
}

/* 通用样式 */
.header {
  background-color: var(--ti-base-color-bg-1, var(--dark-color-bg-primary));
  color: var(--ti-base-color-common-7, var(--dark-color-common-9));
  padding: 20px;
  text-align: center;
  border-bottom: 1px solid var(--ti-common-color-border, var(--dark-color-common-8));
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-buttons {
  display: flex;
  gap: 10px;
}

.main-container {
  display: flex;
  padding: 20px;
}

.sidebar {
  width: 250px;
  padding: 20px;
  background-color: var(--ti-base-color-bg-4, var(--dark-color-bg-accent));
  color: var(--ti-base-color-common-7, var(--dark-color-common-9));
  border-right: 1px solid var(--ti-common-color-border, var(--dark-color-common-8));
}

.book-viewer {
  flex: 1;
  margin-right: 20px;
}

.bookmarks {
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  background-color: var(--ti-button-normal-background-color, var(--dark-color-btn-bg));
  color: var(--ti-button-primary-text-color, var(--dark-color-btn-text));
  border: 1px solid var(--ti-button-normal-background-color, var(--dark-color-btn-border));
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: var(--ti-button-normal-hover-bg-color, var(--dark-color-btn-hover-bg));
}

button:active {
  transform: scale(0.95);
}

.content {
  flex: 1;
  padding: 20px;
  background-color: var(--ti-common-color-light, var(--dark-color-bg-primary));
  color: var(--ti-base-color-common-7, var(--dark-color-common-9));
}

/* 阅读区域 */
.reading-area {
  padding: 20px;
  background-color: var(--ti-common-color-light, var(--dark-color-bg-accent));
  color: var(--ti-base-color-common-7, var(--dark-color-common-9));
  border-radius: 8px;
  font-size: var(--reading-font-size, 14px);
}

.reading-area p {
  margin: 0;
  line-height: 1.6;
}

.content-buttons {
  margin-top: 20px;
  display: flex;
  gap: 10px;
}

.footer {
  padding: 20px;
  text-align: center;
  background-color: var(--ti-base-color-bg-1, var(--dark-color-bg-primary));
  color: var(--ti-base-color-common-7, var(--dark-color-common-9));
}
</style>
