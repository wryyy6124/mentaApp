<template>
  <div class="modalWindowDel">
    <div class="modalWindowDel__body">
      <div class="modalWindowDel__body__contents">
        <div class="modalWindowDel__body__closeBtn" @click="$emit('modalClose')">Close<i class="fas fa-times"></i></div>
        <div class="modalWindowDel__body__header">▼指定の商品を削除する</div>
        <div class="modalWindowDel__body__wrapper">
          <div class="modalWindowDel__body__box">
            <div class="modalWindowDel__body__productImage">Dummy</div>
          </div>
          <div class="modalWindowDel__body__box">
            <div class="modalWindowDel__body__productName">商品名:{{ info.name }}</div>
            <div class="modalWindowDel__body__productPrice">単価:{{ amountDelimiter(info.price) }} 円</div>
            <div class="modalWindowDel__body__number">個数:{{ info.num }}</div>
            <div class="modalWindowDel__body__button" @click="onClickDelete">削除する</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modalWindowDel',
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
    onClickDelete: function() {
      // 親コンポーネント（App.vue）にモーダルウィンドウをcloseさせるように合図
      this.$emit('modalClose');

      // 親コンポーネント（App.vue）に変更したIDを伝える
      this.$emit('onDelete', this.info.id);
    },

    // 価格を3桁区切り表記へ変換する
    amountDelimiter: function(price) { return Number(price).toLocaleString(); },
  },
}
</script>

<style lang="scss">
.modalWindowDel {
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
      background-color: #6986fa;
      border-color: #2850f1;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      margin: 0 auto;
      padding: 10px;

      &:hover { opacity: .4; }
    }
  }
}
</style>
