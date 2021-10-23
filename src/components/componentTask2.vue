<template>
  <div class="funclistArea">
    <div class="listCreate">
      <button class="listCreate__button" :class="{ is_deactive: btnChange }" @click="listCreate(listLength)">
        リスト作成
      </button>
      <button class="listCreate__button" @click="listLengthInit">
        初期化
      </button>
    </div>
    <div class="listSelect">
      <div class="listSelect__header">
        リスト数
      </div>
      <div class="listSelect__number">
        <input type="number" v-model="listLength" @click="numberJudgement(listLength)">
      </div>
    </div>

    <!-- リスト数を0未満に指定した場合のみ出現 -->
    <div :class="{ errMessage: errFlag }" v-if="errFlag">
      マイナスは選べません
    </div>

    <div class="createResult">
      <div class="createResult__header">
        リスト
      </div>
      <ul class="createResult__list">
        <li v-for="(list, index) in lists" :key="list">
          サンプル {{ index + 1 }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'componentTask2',
  data: function() {
    return {
      // リスト生成
      listLength: 0, // リストの長さ（初期値：0）
      lists: {}, // forループ
      errFlag: false, // エラーメッセージ切替
      btnChange: false, // 生成ボタン切替
    }
  },
  methods: {
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
  },
}
</script>

<style lang="scss">
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
</style>