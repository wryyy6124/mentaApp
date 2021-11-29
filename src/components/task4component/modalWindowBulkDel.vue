<template>
  <div class="modalWindowBulk">
    <div class="modalWindowBulk__body">
      <div class="modalWindowBulk__body__contents">
        <div class="modalWindowBulk__body__closeBtn" @click="$emit('modalClose')">Close<i class="fas fa-times"></i></div>
        <div class="modalWindowBulk__body__header">
          <div class="modalWindowBulk__body__text">▼注文の商品数を一括変更する</div>
        </div>
        <div class="modalWindowBulk__body__section">
          <div class="modalWindowBulk__body__wrapper">
            <div class="modalWindowBulk__body__box">
              <div class="modalWindowBulk__body__number">個数：　<input type="number" v-model="num"></div>
              <div class="modalWindowBulk__body__button" @click="onChangeNumBulk">確定する</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modalWindowBulkDel',
  data: function() {
    return {
      num: 1,
    }
  },
  methods: {
    onChangeNumBulk: function() {
      // 個数1未満を指定していたら処理を行わなずに数値を初期化
      if (this.num <= 0) {
        alert('1以上の数値を入力してください');
        this.num = 1;

        return;
      }

      // 親コンポーネント（App.vue）にモーダルウィンドウをcloseさせるように合図
      this.$emit('modalClose');

      // 親コンポーネント（App.vue）に変更したIDと個数を伝える
      this.$emit('changeNumBulk', this.num);
    },
  },
}
</script>

<style lang="scss">
.modalWindowBulk {
  animation-name: displayToggleAnime;
  animation-duration: 1s;

  &__body {
    background-color: rgba(0, 0, 0, .5);
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    overflow: auto;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;

    &__contents {
      background-color: #f4f4f4;
      box-shadow: 0 5px 8px 0 rgba(0, 0, 0, .2),
                  0 7px 20px 0 rgba(0, 0, 0, .2);
      border-radius: 10px;
      position: relative;
      padding: 30px;
      width: 30%;
    }

    &__closeBtn {
      color: #fff;
      cursor: pointer;
      font-size: 25px;
      display: flex;
      align-items: center;
      transition: .7s;
      position: absolute;
      right: 0;
      top: -50px;

      > i { 
        font-size: 1.5em;
        margin-left: 10px;
      }

      &:hover { color: #e4ed63; }
    }

    &__header {
      margin-bottom: 30px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    &__text {
      font-size: 25px;
      font-weight: bold;
    }

    &__button {
      background-color: #f0585d;
      border-color: #ec232a;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      padding: 10px;

      &:hover { opacity: .4; }
    }

    &__wrapper {
      padding: 10px 30px;
      &:not(:last-of-type) { border-bottom: 1px dotted #343434; }
    }

    &__box {
      display: flex;
      justify-content: space-evenly;
      align-items: center;
    }

    &__number {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 50%;

      > input {
        border: 1px solid #000;
        text-align: center;
        margin: 0 auto;
        padding: 10px;
        width: 50%;
      }
    }
  }
}
</style>
