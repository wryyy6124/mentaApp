<template>
  <table class="dataCartTable__body">
    <tr class="dataCartTable__header">
      <th></th>
      <th>商品名</th>
      <th>数量</th>
      <th>単価</th>
      <th>小計</th>
      <th>操作</th>
    </tr>
    <tr class="dataCartTable__list">
      <td class="dataCartTable__check">
        <input type="checkbox">
      </td>
      <td class="dataCartTable__pName">
        サンプル商品
      </td>
      <td class="dataCartTable__num">
        1
      </td>
      <td class="dataCartTable__price">
        1,200
      </td>
      <td class="dataCartTable__total">
        1,200
      </td>
      <td class="dataCartTable__btn">
        <div class="dataCartTable__btn__edit">編集</div>
        <div class="dataCartTable__btn__delete">削除</div>
      </td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'task4TableList',
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