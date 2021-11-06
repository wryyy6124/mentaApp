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
      <th>消費税</th>
      <td>{{ amountDelimiter(calcTax()) }}</td>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>合計</th>
      <td>{{ amountDelimiter(calcPrice() + calcTax()) }}</td>
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
    calcPrice: function() {
      let total = 0

      this.lists.forEach(list => {
        total = total + (list.price * list.num);
      });

      return total;
    },

    calcTax: function() {
      return Math.floor(this.calcPrice() / this.TAX);
    },

    amountDelimiter: function(price) {
      return Number(price).toLocaleString();
    },
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

  }
}

</style>