<template>
    <tr class="dataCartTable__list">
      <td class="dataCartTable__check">
        <input type="checkbox" :checked="checkAll" @change="$emit('chkboxAllDelete')">
      </td>
      <td class="dataCartTable__pName">
        {{ list.name }}
      </td>
      <td class="dataCartTable__num" value="1">
        {{ list.num }}
      </td>
      <td class="dataCartTable__price">
        {{ amountDelimiter(list.price) }}
      </td>
      <td class="dataCartTable__total">
        {{ amountDelimiter(list.price * list.num) }} 円
      </td>
      <td class="dataCartTable__btn">
        <div class="dataCartTable__btn__edit" @click="onClickChangeNum">編集</div>
        <div class="dataCartTable__btn__delete" @click="onClickDeleteList">削除</div>
      </td>
    </tr>
</template>

<script>
export default {
  name: 'task4List',
  props: {
    cart: Object,
    checkAll: Boolean,
  },
  data: function() {
    return {
      // 親コンポーネント（componentTask4.vue）から渡ってきた配列とオプションを格納
      list: this.cart,
      chkAll: this.checkAll,
    }
  },
  methods: {
    // 編集ボタン（クリックした対象の各データを親コンポーネントへ伝達し個数の変更を行われる流れ）
    onClickChangeNum: function() {
      this.$emit('modalOpen',　this.list.id, this.list.name, this.list.num, this.list.price);
    },

    // 削除ボタン（アラートにOK返答するとクリックした対象のIDを親コンポーネントへ伝達し削除処理が行われる流れ）
    onClickDeleteList: function() {
      // 削除するかしないか確認ダイアログが出現
      const resuponse = confirm('本当に削除しますか？');

      // ダイアログで「OK」返答（削除実行の為、削除対象のIDを親へ伝達）
      resuponse ? this.$emit('tableListDelete',　this.list.id) : '';
    },

    // 価格を3桁区切り表記へ変換する
    amountDelimiter: function(price) {
      return Number(price).toLocaleString();
    },
  },
}
</script>

<style lang="scss">

.dataCartTable {
  &__body {
    border-radius: 5px;
    width: 100%;
  }

  &__header {
    background-color: #f0f0f0;
    border-bottom: 1px solid #bbb;

    th {
      text-align: center;
      padding: 15px;
      &:not(:last-of-type) { border-right: 1px solid #bbb; }
    }
  }

  &__list {
    &:not(:last-of-type) { border-bottom: 1px dotted #ccc; }

    td {
      vertical-align: middle;
      padding: 15px;
      &:not(:last-of-type) { border-right: 1px solid #bbb; }
    }
  }

  &__btn {
    display: flex;
    justify-content: space-between;

    &__edit,
    &__delete {
      border-width: 1px;
      border-style: solid;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      font-weight: bold;
      padding: 10px;
      transition: .4s;
      width: 48%;

      &:hover { opacity: .5; }
    }

    &__edit {
      background-color: #f0585d;
      border-color: #ec232a;
    }
    &__delete {
      background-color: #6986fa;
      border-color: #2850f1;
    }
  }
}

</style>