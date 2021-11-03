<template>
    <tr class="dataCartTable__list">
      <td class="dataCartTable__check">
        <input type="checkbox" disabled="disabled">
      </td>
      <td class="dataCartTable__pName">
        {{ list.name }}
      </td>
      <td class="dataCartTable__num">
        {{ number }}
      </td>
      <td class="dataCartTable__price">
        {{ Number(list.price).toLocaleString() }}
      </td>
      <td class="dataCartTable__total">
        <!-- {{ Number(list.price * number).toLocaleString() }} -->
      </td>
      <td class="dataCartTable__btn">
        <div class="dataCartTable__btn__edit" @click="onClickChangeNum">編集</div>
        <div class="dataCartTable__btn__delete">削除</div>
      </td>
    </tr>
</template>

<script>
export default {
  name: 'task4List',
  props: {
    cart: Object,
    changeNum: Number,
  },
  data: function() {
    return {
      // 親コンポーネント（componentTask4.vue）から渡ってきた配列を格納
      list: this.cart,

      // 商品数
      number: this.changeNum,
    }
  },
  methods: {
    onClickChangeNum: function() {
      this.$emit('modalOpen',　this.list.name, this.list.num, this.list.price);
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