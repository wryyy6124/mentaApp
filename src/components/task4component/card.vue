<template>
  <li>
    <div class="dataProduct__image">Dummy</div>
    <div class="dataProduct__body">
      <!-- 親コンポーネントからデータ受領 -->
      <div class="dataProduct__name">{{ list.name }}</div>
      <div class="dataProduct__detail">{{ list.detail }}</div>
      <div class="dataProduct__price">{{ Number(list.price).toLocaleString() }}</div>
      <div class="dataProduct__button">
        <div class="dataProduct__add" @click="$emit('add')">Add to Cart</div>
        <div class="dataProduct__delete" @click="$emit('del')">Delete</div>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: 'card',
  props: {
    product: Object,
  },
  data: function() {
    return {
      // 親コンポーネント（componentTask4.vue）から渡ってきた配列を格納
      list: this.product,
    }
  },
}
</script>

<style lang="scss">
.dataProduct {
  &__list {
    align-items: flex-start;

    > li {
      border: 1px solid #bbb;
      border-radius: 5px;
      flex-basis: 31%;

      &:not(:nth-of-type(3n)) { margin-right: 3.5%; }
      &:last-of-type { margin-right: 0; }
      &:not(:nth-last-of-type(-n+3)) { margin-bottom: 30px; }
    }
  }

  &__image {
    background-color: #666;
    border-radius: 5px 5px 0 0;
    color: #fff;
    text-align: center;
    padding: 50px 0;
  }

  &__body {
    padding: 15px 10px;
    > div:not(:last-of-type) { margin-bottom: 20px; }
  }

  &__name, &__detail {
    line-height: 1.5;
    padding-left: 10px;
  }

  &__detail { font-size: 75%; }
  &__name, &__price { font-weight: bold; }

  &__price {
    color: #df0000;
    text-align: center;

    &::before {
      content: '¥';
      font-size: 80%;
      display: inline-block;
      margin-right: 5px;
    }
  }

  &__button {
    display: flex;
    justify-content: space-between;
    align-items: center;

    > div {
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      font-size: 80%;
      text-align: center;
      transition: .4s;
      padding: 10px 5px;
      flex-basis: 48%;

      &:hover { opacity: .5; }
    }
  }

  &__add { background-color: #fd7e00; }
  &__delete { background-color: #c00; }
}
</style>