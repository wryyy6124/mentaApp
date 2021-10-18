<template>
  <!-- #app start -->
  <div id="app">
    <!-- FontAwesome CDN読み込み -->
    <link rel ="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">

    <!-- 完了した課題分のアコーディオン開閉 -->
    <div class="taskOpen" v-on:click="toggleAccordion">{{ accordionMsg }} <i class="fas fa-angle-double-down"></i></div>

    <!-- 完了した課題を格納する -->
    <div class="accordionTask" v-bind:class="{ is_display: isDisplay }" v-if="isDisplay">
      <!-- 加算・減算 start -->
      <div class="funcWrapper">
        <div class="funcWrapper__header">①数値の加算・減算・初期化</div>
        <div class="funcCalcArea">
          <div class="selectCalc">
            <button class="selectCalc__button" v-on:click="clickCalc(1)">プラス</button>
            <button class="selectCalc__button" v-on:click="clickCalc(-1)">マイナス</button>
            <button class="selectCalc__button" v-on:click="calcInit">初期化</button>
          </div>
          <div class="resultCalc">
            <div class="resultCalc__header">結果</div>
            <div class="resultCalc__body">{{ calc }}</div>
          </div>
        </div>
      </div>
      <!-- 加算・減算　end -->

      <!-- リスト生成　start -->
      <div class="funcWrapper">
        <div class="funcWrapper__header">②リスト生成・初期化</div>
        <div class="funclistArea">
          <div class="listCreate">
            <button class="listCreate__button" v-bind:class="{ is_deactive: btnChange }" v-on:click="listCreate(listLength)">リスト作成</button>
            <button class="listCreate__button" v-on:click="listLengthInit">初期化</button>
          </div>
          <div class="listSelect">
            <div class="listSelect__header">リスト数</div>
            <div class="listSelect__number">
              <input type="number" v-model="listLength" v-on:click="numberJudgement(listLength)">
            </div>
          </div>

          <!-- リスト数を0未満に指定した場合のみ出現 -->
          <div v-bind:class="{ errMessage: errFlag }" v-if="errFlag">マイナスは選べません</div>

          <div class="createResult">
            <div class="createResult__header">リスト</div>
            <ul class="createResult__list">
              <li v-for="(list, index) in lists" v-bind:key="list">サンプル {{ index + 1 }}</li>
            </ul>
          </div>
        </div>
      </div>
      <!-- リスト生成　end -->
    </div>

    <!-- コメント投稿　start -->
    <div class="funcWrapper">
      <div class="funcWrapper__header">③コメントの投稿／削除・検索</div>
      <div class="funcCommentArea">
        <div class="commentPost">
          <div class="commentPost__header">コメント</div>
          <div class="commentPost__body">
            <input type="text" v-model="commentPost" name="commentPost" class="commentPost__inputText" placeholder="コメントを入力してください">
            <button class="commentPost__executionBtn" v-on:click="clickPost">追加</button>
          </div>
        </div>

        <div class="commentSearch">
          <div class="commentSearch__body">
            <div class="commentSearch__executionIcon"><i class="fas fa-search"></i></div>
            <input type="text" v-model.trim="searchText" v-on:input="searchTextFunc(searchText)" name="commentSearch" class="commentSearch__inputText" placeholder="コメント一覧から検索したいキーワードを入力してください">
          </div>
        </div>

        <!-- 検証用 -->
        <div>{{ searchText }}</div>

        <div class="commentList">
          <div class="commentList__header">コメント</div>
          <div class="commentList__postBody" v-for="(comment, index) in commentValues" v-bind:key="index">
            <div class="commentList__postText">{{ comment }}</div>
            <div class="commentList__postButton">
              <div class="commentList__postButton__inner" v-on:click="commentDelete(index)">削除</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- コメント投稿 end -->

  </div>
  <!-- #app end -->
</template>

<script>
export default {
  name: 'App',
  data: function() {
    return {
      // 課題（完了分）の格納
      isDisplay: false,
      accordionMsg: '課題完了分（クリックで開閉）',

      // 加算・減算 
      calc: 0, // 演算値（初期値：0）

      // リスト生成
      listLength: 0, // リストの長さ（初期値：0）
      lists: {}, // forループ
      errFlag: false, // エラーメッセージ切替
      btnChange: false, // 生成ボタン切替

      // コメント投稿
      commentPost: '',
      commentValues: [],
      searchText: '',
    }
  },
  methods: {
    /*---------------
      課題（完了分）の格納
    ---------------*/
    toggleAccordion: function() { this.isDisplay = !this.isDisplay; },

    /*---------------
      加算・減算
    ---------------*/
    // プラス or マイナスボタン押下時 => calcプロパティに対して加算or減算の処理を行なう
    clickCalc: function(num) { this.calc += num; },

    // 初期化ボタン押下時の処理 => calcプロパティを初期値に設定する
    calcInit: function() { this.calc = 0; },

    /*---------------
      リスト生成
    ---------------*/
    // リスト作成ボタン押下時の処理 => 入力された数値分の配列を作成する
    listCreate: function(len) { this.lists = new Array(Number(len)); },

    // 初期化ボタン押下時の処理 => lists/listLengthプロパティへ初期値を設定する
    listLengthInit: function() {
      // 各プロパティの値とオブジェクトの内容をクリアする
      this.listLength = 0;
      this.lists = {},

      // リスト作成ボタンを活性化させる
      this.errFlag = false;
      this.btnChange = false;
    },

    // リスト数の入力値判定
    numberJudgement: function(num) {
      // 入力値0未満の場合はリスト作成ボタンを非活性化させる
      if(Number(num) < 0) {
        this.errFlag = true;
        this.btnChange = true;

        return true;
      }

      // 入力値0以上の場合はリスト作成ボタンを活性化させる
      this.errFlag = false;
      this.btnChange = false;

      return false;
    }, 

    /*---------------
      コメント投稿
    ---------------*/
    // コメント投稿
    clickPost: function() {
      // コメント入力欄の判定
      if(this.commentPost === ''){
        alert('コメント投稿には1文字以上の入力が必要です');
        return false;
      }

      // 一覧にコメント追加
      this.commentValues.push(this.commentPost);

      // コメント入力欄を初期化
      this.commentPost = '';
    },

    // コメント削除
    commentDelete: function(num) { this.commentValues.splice(num, 1); },

    // ▼リスト数の入力値判定
    searchTextFunc: function(txt) {
      console.log(txt);
      
    },

  }
}
</script>

<style lang="scss">

#app {
  color: #2c3e50;
  font-size: 20px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.taskOpen {
  background-color: #2c3e50;
  color: #fff;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
  margin-bottom: 40px;
  padding: 15px 0;
  width: 750px;
}

/*---------------------------------------
  各セクションの入れ物（.funcWrapper）のスタイル定義
---------------------------------------*/
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

/*---------------------------------------
  「計算処理」エリアのスタイル定義
---------------------------------------*/
.funcCalcArea {
  margin: 0 auto;
  padding: 30px 0;
  width: 500px;

  > div {
    display: flex;
    justify-content: end;
    align-items: center;

    &:not(:last-of-type) { margin-bottom: 25px; }
  }
}

.selectCalc {
  &__button {
    cursor: pointer;
    padding: 10px 0;
    flex-basis: 31%;

    &:not(:first-of-type) { margin-left: 3.5%; }
  }
}

.resultCalc {
  > div {
    &:not(:first-of-type) { margin-left: 3.5%; }
  }

  &__header { 
    text-align: right;
    width: 30%;
  }

  &__body {
    border: 1px solid #666;
    text-align: center;
    padding: 10px;
    width: 30%;
  }
}

/*---------------------------------------
  「リスト生成処理」エリアのスタイル定義
---------------------------------------*/
.funclistArea {
  margin: 0 auto;
  padding: 30px 0;
  width: 500px;

  > div {
    &:not(:last-of-type) { margin-bottom: 25px; }
  }
}

.listCreate {
  display: flex;
  justify-content: end;
  align-items: center;

  &__button {
    background-color: #ddd;
    border: 1px solid #666;
    border-radius: 5px;
    color: #232323;
    cursor: pointer;
    font-weight: bold;
    transition: .4s;
    padding: 10px 0;
    flex-basis: 31%;

    &:not(:first-of-type) { margin-left: 3.5%; }

    &.is_deactive {
      background-color: #ddd;
      border: 1px solid #bbb;
      color: #bbb;
      cursor: default;
      font-weight: normal;
      opacity: .8;
      pointer-events: none;
    }
  }
}

.listSelect {
  display: flex;
  justify-content: end;
  align-items: center;

  > div {
    &:not(:first-of-type) { margin-left: 3.5%; }
  }

  &__header {
    text-align: right;
    width: 30%;
  }

  &__number {
    border: 1px solid #666;
    text-align: center;
    width: 15%;

    > input {
      border: none;
      font-size: 20px;
      text-align: center;
      padding: 10px 0;
      width: 100%;
    }
  }
}

.createResult {
  border: 1px solid #bbb;
  border-radius: 5px;
  text-align: center;

  &__header {
    background-color: #f0f0f0;
    border-radius: 5px 5px 0 0;
    padding: 10px 0;
    width: 100%;
  }

  &__list {
    list-style: none;
    margin: 0;
    padding: 0;

    > li {
      border-top: 1px solid #aaa;
      padding: 10px 0;
    }
  }
}

.errMessage {
  background-color: #f1d9d9;
  border: 1px solid #500;
  border-radius: 5px;
  color: #500;
  transition: .4s;
  text-align: center;
  padding: 10px 0;
}

/*---------------------------------------
  「コメント投稿」エリアのスタイル定義
---------------------------------------*/

.funcCommentArea {
  margin: 0 auto;
  padding: 30px 0;
  width: 750px;

  > div {
    &:not(:last-of-type) { margin-bottom: 20px; }
  }
}

.commentPost {
  display: flex;
  justify-content: end;
  align-items: center;

  &__header {
    text-align: right;
    padding-right: 15px;
    width: 20%;
  }

  &__body {
    display: flex;
    justify-content: end;
    align-items: center;
    width: 80%;
  }

  &__inputText,
  &__executionBtn {
    // ボタンとテキストのスタイルを一旦打ち消す
    appearance: none;
    background-color: transparent;
    border: 0;
    border-radius: 0;
    box-sizing: content-box;
    color: inherit;
    padding: 0;

    // 共通スタイル指定
    border: 1px solid #bbb;
    font-size: 16px;
    line-height: 1.2;
    padding: 15px;
  }

  &__inputText {
    outline: none;
    width: 90%;

    &::placeholder { color: #ddd; }
  }

  &__executionBtn {
    background-color: #dfdfdf;
    border-radius: 5px;
    cursor: pointer;
    text-align: center;
    transition: .4s;
    margin-left: 10px;
    width: 10%;

    &:hover { background-color: #ccc; }
  }
}

.commentSearch {
  display: flex;
  justify-content: end;
  align-items: center;

  &__body {
    display: flex;
    justify-content: end;
    align-items: center;
    width: 90%;
  }

  &__inputText,
  &__executionIcon {
    // ボタンとテキストのスタイルを一旦打ち消す
    appearance: none;
    background-color: transparent;
    border: 0;
    border-radius: 0;
    box-sizing: content-box;
    color: inherit;
    padding: 0;

    // 共通スタイル指定
    border: 1px solid #bbb;
    font-size: 16px;
    line-height: 1.2;
    padding: 15px;
  }

  &__executionIcon {
    background-color: #dfdfdf;
    border-radius: 5px 0 0 5px;
    text-align: center;
    transition: .4s;
  }

  &__inputText {
    border-left: none;
    outline: none;
    width: 100%;

    &::placeholder { color: #ddd; }
  }
}

.commentList {
  border: 1px solid #bbb;

  &__header {
    background-color: #f0f0f0;
    text-align: center;
    padding: 10px 0;
  }

  &__postBody {
    border-top: 1px solid #bbb;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__postText {
    border-right: 1px solid #bbb;
    display: flex;
    align-items: center;
    padding: 10px 25px 10px 25px;
    width: 75%;
    min-height: 50px;
  }

  &__postButton {
    width: 25%;

    &__inner {
      background-color: #f11818;
      border-radius: 10px;
      color: #fff;
      cursor: pointer;
      font-weight: bold;
      text-align: center;
      transition: .2s;
      margin: 0 auto;
      padding: 10px;
      width: 75%;

      &:hover { opacity: .7; }
    }
  }
}
</style>
