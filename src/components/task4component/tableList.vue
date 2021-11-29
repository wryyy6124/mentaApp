<template>
    <tr class="dataCartTable__list">
      <td class="dataCartTable__check"><input type="checkbox" :checked="list.chk" @change="onChangeCheckBox"></td>
      <td class="dataCartTable__pName">{{ list.name }}</td>
      <td class="dataCartTable__num">{{ list.num }}</td>
      <td class="dataCartTable__price">{{ amountDelimiter(list.price) }}</td>
      <td class="dataCartTable__total">{{ amountDelimiter(list.price * list.num) }} 円</td>
      <td class="dataCartTable__btn">
        <div class="dataCartTable__btn__edit" @click="onClickChangeNum">編集</div>
        <div class="dataCartTable__btn__delete" @click="onClickDeleteList" v-if="list.chk">削除</div>
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
    // 編集ボタン押下
    onClickChangeNum: function() {
      // クリックした対象の各データを親コンポーネントへ伝達し個数変更処理を行なう
      this.$emit('modalOpen',　this.list);
    },

    // 削除ボタン押下
    onClickDeleteList: function() {
      // 削除するかしないか確認ダイアログが出現
      // const response = confirm('本当に削除しますか？');

      // ダイアログで「OK」返答すると削除実行の為、削除対象のIDを親へ伝達し処理を行なう
      // response ? this.$emit('tableListDelete',　this.list.id) : '';

      this.$emit('tableListDelete',　this.list);
    },

    // チェックボックスの付け外しで発生するイベント
    onChangeCheckBox: function() {
      // フラグの切り替え
      this.list.chk = !this.list.chk;

      // 親コンポーネントへ指令を出す
      this.$emit('chkboxSwitch');
    },

    // 引数で受け取った数値（価格）を3桁区切り表記へ変換する
    amountDelimiter: function(price) { return Number(price).toLocaleString(); },
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
      padding: 15px 10px;
      &:not(:last-of-type) { border-right: 1px solid #bbb; }
    }
  }

  &__btn {
    display: flex;
    justify-content: center;

    > div {
      &:nth-of-type(2) { margin-left: 6%; }
    }

    &__edit,
    &__delete {
      border-width: 1px;
      border-style: solid;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      font-size: 15px;
      font-weight: bold;
      padding: 10px;
      transition: .4s;
      width: 44%;

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