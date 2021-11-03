<template>
  <table class="dataCartTable__calc">
    <tr class="dataCartTable__calc__row">
      <th></th>
      <th>金額</th>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>商品金額</th>
      <td>1,200</td>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>消費税</th>
      <td>120</td>
    </tr>
    <tr class="dataCartTable__calc__row">
      <th>合計</th>
      <td>1,320</td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'task4Calc',
  props: {
  },
  data: function() {
    return {
    }
  },
  methods: {
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