<template>
  <!-- #app start -->
  <div id="app">
    <!-- CDN読み込み -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/destyle.css@1.0.15/destyle.css">
    <link rel ="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">

    <!-- 完了した課題を格納する -->
    <div class="complateTask">
      <div class="taskToggle" @click="toggleAccordion">タスク完了分（クリックで開閉）<i class="fas fa-angle-double-down"></i></div>
      <div class="accordionTask" :class="{ is_display: isDisplay.task }">
        <div class="funcWrapper">
          <div class="funcWrapper__header">①数値の加算・減算・初期化</div>
          <!-- 加算・減算　 -->
          <componentTask1 />
        </div>

        <div class="funcWrapper">
          <div class="funcWrapper__header">②リスト生成・初期化</div>
          <!-- リスト生成　-->
          <componentTask2 />
        </div>

        <div class="funcWrapper">
          <div class="funcWrapper__header">③コメントの投稿／削除・検索</div>
          <!-- コメント投稿 -->
          <componentTask3 />
        </div>

        <div class="funcWrapper">
          <div class="funcWrapper__header">④コンポーネントへ値（プロパティ）引き渡し</div>
          <!-- 子コンポーネント引き継ぎ -->
          <componentTask4 />
        </div>
      </div>
    </div>

    <!-- 進行中の課題を格納する -->
    <div class="progressTask">
      <div class="funcWrapper">
        <div class="funcWrapper__header">⑤カテゴリー分け</div>
        <!-- カテゴリー分け -->
        <componentTask5 />
      </div>
    </div>

  </div>
  <!-- #app end -->

</template>

<script>
// 各課題コンポーネントのインポート
import componentTask1 from "./components/componentTask1";
import componentTask2 from "./components/componentTask2";
import componentTask3 from "./components/componentTask3";
import componentTask4 from "./components/componentTask4";
import componentTask5 from "./components/componentTask5";

export default {
  name: 'App',
  components: {
    componentTask1, componentTask2,
    componentTask3, componentTask4,
    componentTask5,
  },
  data: function() {
    return {
      // 課題（完了分）の格納
      isDisplay: {
        // 完了分課題
        task: false,
      },
    }
  },
  methods: {
    // 課題（完了分）の格納
    toggleAccordion: function() { this.isDisplay.task = !this.isDisplay.task; },
  },
}
</script>

<style lang="scss">
#app {
  color: #2c3e50;
  font-size: 20px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding-top: 30px;
}

@keyframes displayToggleAnime {
  from { opacity: 0; }
  to { opacity: 1; }
}

.taskToggle {
  background-color: #2c3e50;
  color: #fff;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
  padding: 15px 0;
  width: 980px;
}

.accordionTask {
  animation-name: displayToggleAnime;
  animation-duration: 1s;
  display: none;
  &.is_display { display: block; }
  > div { background-color: #f0fafa; }
}

// 各セクションの入れ物（.funcWrapper）のスタイル定義
.funcWrapper {
  margin: 0 auto;
  padding: 25px;
  padding-bottom: 60px;
  width: 980px;

  > div {
    > div {
      margin: 0 auto;
      width: 750px;
      &:not(:last-of-type) { margin-bottom: 40px; }
    }
  }

  &__header {
    border-bottom: 5px solid;
    font-weight: bold;
    margin-bottom: 30px;
    padding: 10px 0;
  }
}

</style>