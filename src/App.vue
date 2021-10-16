<template>
  <!-- #app start -->
  <div id="app">

    <!-- 加算・減算 start -->
    <div class="funcWrapper">
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
      <div class="funclistArea">
        <div class="listCreate">
          <button class="listCreate__button" v-on:click="listCreate(listLength)">リスト作成</button>
          <button class="listCreate__button" v-on:click="listLengthInit">初期化</button>
        </div>
        <div class="listSelect">
          <div class="listSelect__header">リスト数</div>
          <div class="listSelect__number">
            <input type="number" v-model="listLength" v-on:click="numberJudgement(listLength)">
          </div>
        </div>

        <!-- リスト数を0未満に指定した場合のみ出現 -->
        <div class="errMessage" v-if="errFlag">マイナスは選べません</div>

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
  <!-- #app end -->
</template>

<script>
export default {
  name: 'App',
  data: function() {
    return {
      // 加算・減算 
      calc: 0,

      // リスト生成
      listLength: 0,
      lists: {},
      errFlag: false,
    }
  },
  methods: {
    /*---------------
      加算・減算
    ---------------*/
    // ▼プラス or マイナスボタン押下時 => calcプロパティに対して加算or減算の処理を行なう
    clickCalc: function(num) { this.calc += num; },

    // ▼初期化ボタン押下時の処理 => calcプロパティを初期値に設定する
    calcInit: function() { this.calc = 0; },

    /*---------------
      リスト生成
    ---------------*/
    // ▼リスト作成ボタン押下時の処理 => 入力された数値分の配列を作成する
    listCreate: function(len) { this.lists = new Array(Number(len)); },

    // ▼初期化ボタン押下時の処理 => lists/listLengthプロパティへ初期値を設定する
    listLengthInit: function() {
      // 値のクリア
      this.listLength = 0;
      this.lists = {},

      // リスト作成ボタンを活性化させる
      this.errFlag = false;
      document.querySelector('.listCreate__button').classList.remove('is_deactive');
    },

    // ▼リスト数の入力値判定
    numberJudgement: function(num) {
      // リスト作成ボタンのDOM取得
      const $CreateBtn = document.querySelector('.listCreate__button');

      // 入力値0未満の場合はリスト作成ボタンを非活性化させる
      if(Number(num) < 0) {
        this.errFlag = true;
        $CreateBtn.classList.add('is_deactive');

        return true;
      }

      // 入力値0以上の場合はリスト作成ボタンを活性化させる
      this.errFlag = false;
      $CreateBtn.classList.remove('is_deactive');

      return false;
    }, 
  }
}
</script>

<style lang="scss">
body { margin: 0; }

#app {
  color: #2c3e50;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/*---------------------------------------
  各セクションの入れ物（.funcWrapper）のスタイル定義
---------------------------------------*/
.funcWrapper {
  &:not(:last-of-type) { margin-bottom: 30px; }
}

/*---------------------------------------
  「計算処理」エリアのスタイル定義
---------------------------------------*/
.funcCalcArea {
  font-size: 20px;
  margin: 0 auto;
  padding: 30px 0;
  width: 500px;

  & > div {
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
  & > div {
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
  font-size: 20px;
  margin: 0 auto;
  padding: 30px 0;
  width: 500px;

  & > div{
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

  & > div {
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

    & > input {
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

    & > li {
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
