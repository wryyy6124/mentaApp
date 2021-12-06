<template>
  <div class="funcCalcArea">
    <div class="selectCalc">
      <button class="selectCalc__button" @click="clickCalc(1)">プラス</button>
      <button class="selectCalc__button" @click="clickCalc(-1)">マイナス</button>
      <button class="selectCalc__button" @click="calcInit">初期化</button>
    </div>
    <div class="resultCalc">
      <div class="resultCalc__header">結果</div>
      <div class="resultCalc__body">{{ calc }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'componentTask1',
  data: function() {
    return {
      calc: 0, // 演算値（初期値：0）
    }
  },
  methods: {
    // プラス or マイナスボタン押下時 => calcプロパティに対して加算or減算の処理を行なう
    clickCalc: function(num) { this.calc += num; },

    // 初期化ボタン押下時の処理 => calcプロパティを初期値に設定する
    calcInit: function() { this.calc = 0; },
  },
}
</script>

<style lang="scss">
/*---------------------------------------
  「計算処理」エリアのスタイル定義
---------------------------------------*/
.funcCalcArea {
  > div {
    display: flex;
    justify-content: end;
    align-items: center;

    &:not(:last-of-type) { margin-bottom: 25px; }
  }
}

.selectCalc {
  &__button {
    background-color: #ddd;
    border: 1px solid #666;
    border-radius: 5px;
    color: #232323;
    cursor: pointer;
    text-align: center;
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
</style>
