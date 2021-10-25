<template>
  <!-- #app start -->
  <div id="app">
    <!-- reset.css destyle -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/destyle.css@1.0.15/destyle.css">

    <!-- FontAwesome CDN読み込み -->
    <link rel ="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">

    <!-- 完了した課題分のアコーディオン開閉 -->
    <div class="taskToggle" @click="toggleAccordion">
      {{ accordionMsg }} <i class="fas fa-angle-double-down"></i>
    </div>

    <!-- 完了した課題を格納する -->
    <div class="accordionTask" :class="{ is_display: isDisplay }" v-if="isDisplay">
      <div class="funcWrapper">
        <div class="funcWrapper__header">
          ①数値の加算・減算・初期化
        </div>
        <!-- 加算・減算　 -->
        <componentTask1></componentTask1>
      </div>

      <div class="funcWrapper">
        <div class="funcWrapper__header">
          ②リスト生成・初期化
        </div>
        <!-- リスト生成　-->
        <componentTask2></componentTask2>
      </div>

      <div class="funcWrapper">
        <div class="funcWrapper__header">
          ③コメントの投稿／削除・検索
        </div>
        <!-- コメント投稿 -->
        <componentTask3></componentTask3>
      </div>
    </div>

      <div class="funcWrapper">
        <div class="funcWrapper__header">
          ④コンポーネントへ値（プロパティ）引き渡し
        </div>
        <!-- コメント投稿 -->
        <componentTask4 :products="productList"></componentTask4>
      </div>

  </div>
  <!-- #app end -->
</template>

<script>
// 各コンポーネントのインポート
import componentTask1 from "./components/componentTask1";
import componentTask2 from "./components/componentTask2";
import componentTask3 from "./components/componentTask3";
import componentTask4 from "./components/componentTask4";

export default {
  name: 'App',
  components: {
    componentTask1,
    componentTask2,
    componentTask3,
    componentTask4,
  },
  data: function() {
    return {
      // 課題（完了分）の格納
      isDisplay: false,
      accordionMsg: '課題完了分（クリックで開閉）',

      // 子コンポーネント（Task４）へ受け渡す
      productList: [],
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
  padding-top: 30px;
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
  margin: 0 auto;
  padding-bottom: 60px;
  width: 750px;

  &__header {
    border-bottom: 5px solid;
    font-weight: bold;
    margin-bottom: 30px;
    padding: 10px 0;
  }
}
</style>