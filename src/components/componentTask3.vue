<template>
  <div class="funcWrapper">
    <div class="funcWrapper__header">
      ③コメントの投稿／削除・検索
    </div>
    <div class="funcCommentArea">
      <div class="commentPost">
        <div class="commentPost__header">
          コメント
        </div>
        <div class="commentPost__body">
          <input class="commentPost__inputText"　type="text" v-model="commentText" name="Post" placeholder="コメントを入力してください">
          <button class="commentPost__executionBtn" @click="commentPost">
            追加
          </button>
        </div>
      </div>

      <div class="commentSearch">
        <div class="commentSearch__body">
          <div class="commentSearch__executionIcon">
            <i class="fas fa-search"></i>
          </div>
          <input class="commentSearch__inputText" type="text" v-model.trim="searchText" @keyup="commentFilter" name="commentSearch" placeholder="コメント一覧から検索したいキーワードを入力してください">
        </div>
      </div>

      <div class="commentList">
        <div class="commentList__header">
          コメント
        </div>
        <div class="commentList__postBody" v-for="(comment, index) in commentFilter" :key="index">
          <div class="commentList__postText">
            {{ comment }}
          </div>
          <div class="commentList__postButton">
            <div class="commentList__postButton__inner" @click="commentDelete(index)">
              削除
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'componentTask3',
  data: function() {
    return {
      // コメント投稿・検索・削除
      commentText: '',
      searchText: '',

      commentValues: [],
      commentFilters: [],
    }
  },
  methods: {
    // 投稿
    commentPost: function() {
      // コメント入力欄に1字以上テキストが入っているか
      if(this.commentText !== '') {
        // テキストを配列へ追加後、入力欄を初期化する
        this.commentValues.push(this.commentText);
        this.commentText = '';
      }
    },

    // 検索キーワードと部分一致するコメントを洗い出して一覧へ表示させる
    // 呼び出し元：computedプロパティ内のcommentFilter関数
    findByComment: function(comments, search) {
      return comments.filter(function(comment) {
        // 部分一致のワードを捜索、なお入力がない場合はコメント全件表示させる
        return (comment.indexOf(search) !== -1 || search === '')
      })
    },

    // 削除
    commentDelete: function(num) { this.commentValues.splice(num, 1); },
  },
  computed: {
    // コメントフィルター
    commentFilter: function() {
      // コメント一覧と検索ワードの2点を引数に「method」プロパティ内の「findByComment」メソッドを実行
      return this.findByComment(this.commentValues, this.searchText)
    },
  },
}
</script>

<style lang="scss">
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