<template>
  <!-- #app start -->
  <div id="app">
    <!-- FontAwesome CDN読み込み -->
    <link rel ="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">

    <!-- 完了した課題分のアコーディオン開閉 -->
    <div class="taskToggle" @click="toggleAccordion">
      {{ accordionMsg }} <i class="fas fa-angle-double-down"></i>
    </div>

    <!-- 完了した課題を格納する -->
    <div class="accordionTask" :class="{ is_display: isDisplay }" v-if="isDisplay">
      <!-- 加算・減算 start -->
      <componentTask1></componentTask1>
      <!-- 加算・減算　end -->

      <!-- リスト生成　start -->
      <componentTask2></componentTask2>
      <!-- リスト生成　end -->

      <!-- コメント投稿　start -->
      <componentTask3></componentTask3>
      <!-- コメント投稿 end -->
    </div>
  </div>
  <!-- #app end -->
</template>

<script>
// 各コンポーネントのインポート
import componentTask1 from './components/componentTask1'
import componentTask2 from './components/componentTask2'
import componentTask3 from './components/componentTask3'

export default {
  name: 'App',
  components: {
    componentTask1,
    componentTask2,
    componentTask3,
  },
  data: function() {
    return {
      // 課題（完了分）の格納
      isDisplay: true,
      accordionMsg: '課題完了分（クリックで開閉）',
    }
  },
  methods: {
    // 課題（完了分）の格納
    toggleAccordion: function() { this.isDisplay = !this.isDisplay; },
  },
}
</script>

<style lang="scss">

// Vueインスタンスのスタイル定義
#app {
  color: #2c3e50;
  font-size: 20px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

// 完了分課題収納用のスタイル定義
.taskToggle {
  background-color: #2c3e50;
  color: #fff;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
  margin-bottom: 40px;
  padding: 15px 0;
  width: 750px;
}

// 各セクションの入れ物（.funcWrapper）のスタイル定義
.funcWrapper {
  &:not(:last-of-type) { margin-bottom: 30px; }

  &__header {
    border-bottom: 5px solid;
    font-weight: bold;
    margin: 0 auto;
    padding: 10px 0;
    width: 750px;
  }
}
</style>