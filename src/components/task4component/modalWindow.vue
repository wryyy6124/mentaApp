<template>
  <div class="modalWindow">
    <div class="modalWindow__body">
      <div class="modalWindow__body__contents">
        <div class="modalWindow__body__closeBtn" @click="$emit('modalClose')">
        <!-- <div class="modalWindow__body__closeBtn"> -->
          Close<i class="fas fa-times"></i>
        </div>
        <div class="modalWindow__body__header">
          ▼注文の商品数を変更する
        </div>
        <div class="modalWindow__body__wrapper">
          <div class="modalWindow__body__box">
            <div class="modalWindow__body__productImage">
              Dummy
            </div>
          </div>
          <div class="modalWindow__body__box">
            <div class="modalWindow__body__productName">
              商品名：{{ info.name }}
            </div>
            <div class="modalWindow__body__productPrice">
              単価：{{ Number(info.price).toLocaleString() }} 円
            </div>
            <div class="modalWindow__body__number">
              個数：<input type="number" v-model="info.num">
            </div>
            <div class="modalWindow__body__button" @click="onChangeNum">
              確定する
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modalWindow',
  props: {
    cartData: Object,
  },
  data: function() {
    return {
      info: {
        id: this.cartData.id,
        name: this.cartData.name,
        price: this.cartData.price,
        num: this.cartData.num,
      },
    }
  },
  methods: {
    onChangeNum: function() {
      // 個数1未満を指定していたら処理を行わなずに数値を初期化
      if (this.info.num <= 0) {
        alert('1以上の数値を入力してください');
        this.info.num = 1;

        return;
      }

      // 親コンポーネント（App.vue）にモーダルウィンドウをcloseさせるように合図
      this.$emit('modalClose');

      // 親コンポーネント（App.vue）に変更したIDと個数を伝える
      this.$emit('changeNum', this.info.id, Number(this.info.num));
    },
  },
}
</script>

<style lang="scss">
.modalWindow {
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
      font-size: 25px;
      font-weight: bold;
      margin-bottom: 15px;
    }

    &__wrapper {
      display: flex;
      justify-content: space-between;
    }

    &__box {
      width: 45%;
      > div:not(:last-of-type) { margin-bottom: 15px; }
    }

    &__productImage {
      background-color: #666;
      border-radius: 5px;
      color: #fff;
      text-align: center;
      padding: 50px 0;
    }

    &__number {
      display: flex;
      align-items: center;

      > input {
        border: 1px solid #000;
        padding: 10px;
        width: 50%;
      }
    }

    &__button {
      background-color: #f0585d;
      border-color: #ec232a;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      margin: 0 auto;
      padding: 10px;
      width: 50%;

      &:hover { opacity: .4; }
    }
  }
}
</style>
