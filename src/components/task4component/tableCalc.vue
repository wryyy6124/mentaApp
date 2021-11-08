<template>
  <table class="dataCartTable__calc">
    <tr class="dataCartTable__calc__row">
      <th></th>
      <th>金額</th>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>商品金額</th>
      <td>{{ amountDelimiter(calcPrice()) }}</td>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>消費税（{{ TAX }}%）</th>
      <td>{{ amountDelimiter(calcTax()) }}</td>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>合計代金</th>
      <td class="dataCartTable__calc__total">{{ amountDelimiter(calcPrice() + calcTax()) }} 円</td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'task4Calc',
  props: {
    carts: Array,
  },
  data: function() {
    return {
      // 消費税率10％
      TAX: 10,

      // 親コンポーネント（componentTask4.vue）から渡ってきた配列を格納
      lists: this.carts,
    }
  },
  methods: {
    // カートに追加したアイテムの代金を計算する
    calcPrice: function() {
      let total = 0

      this.lists.forEach(list => {
        total = total + (list.price * list.num);
      });

      return total;
    },

    // カートに追加したアイテムの代金の消費税を計算する
    calcTax: function() { return Math.floor(this.calcPrice() / this.TAX); },

    // 価格を3桁区切り表記へ変換する
    amountDelimiter: function(price) { return Number(price).toLocaleString(); },
  },
}
</script>

<style lang="scss">

.dataCartTable {
  &__calc {
    margin-left: auto;
    width: 40%;

    tr {
      > * {
        width: 50%;
        &:not(:last-child) { border-right: 1px solid #bbb; }
      }

      &:not(:last-of-type) { border-bottom: 1px solid #bbb; }
    }

    th {
      background-color: #f0f0f0;
      text-align: center;
    }
    th, td { padding: 10px; }

    &__total {
      color: #df0000;
      font-size: 1.2em;
      font-weight: bold;
    }
  }
}

</style>