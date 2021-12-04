<template>
  <div class="modalWindowBulkDel">
    <div class="modalWindowBulkDel__body">
      <div class="modalWindowBulkDel__body__contents">
        <div class="modalWindowBulkDel__body__closeBtn" @click="$emit('modalClose')">Close<i class="fas fa-times"></i></div>
        <div class="modalWindowBulkDel__body__header">
          <div class="modalWindowBulkDel__body__text">▼チェック済みの商品を一括削除する</div>
        </div>
        <div class="modalWindowBulkDel__body__section">
          <div class="modalWindowBulkDel__body__wrapper">
            <ol class="modalWindowBulkDel__body__list">
              <li v-for="item in items">{{ item.name }}</li>
            </ol>
          </div>
          <div class="modalWindowBulkDel__body__box">
            <div class="modalWindowBulkDel__body__button" @click="onChangeNumBulk">全て削除する</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modalWindowBulkDelDel',
  props: {
    chkCarts: Array,
  },
  data: function() {
    return {
      items: this.chkCarts,
    }
  },
  methods: {
    onChangeNumBulk: function() {
      // 親コンポーネント（App.vue）にモーダルウィンドウをcloseさせるように合図
      this.$emit('modalClose');
    },
  },
}
</script>

<style lang="scss">
.modalWindowBulkDel {
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
      background-color: #6986fa;
      border-color: #2850f1;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      padding: 10px;

      &:hover { opacity: .4; }
    }

    &__list {
      background-color: #ddd;
      list-style: auto;
      overflow: scroll;
      margin-bottom: 30px;
      padding: 20px 0 20px 50px;
      max-height: 200px;

      > li {
        padding-left: 15px;
        &:not(:last-of-type) { margin-bottom: 25px; }
      }
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
