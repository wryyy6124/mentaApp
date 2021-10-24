<template>
  <div class="funcPropGiveArea">
    <div class="dataGive">
      <div class="dataGive__header">
        データ追加
      </div>
      <div class="dataGive__body">
        <ul class="dataGive__body__list">
          <li>
            <span class="dataGive__list__header">商品名</span>
            <input class="dataGive__list__inputText"　type="text" name="Product" placeholder="商品名">
          </li>
          <li>
            <span class="dataGive__list__header">詳細</span>
            <input class="dataGive__list__inputText"　type="text" name="Product" placeholder="商品詳細">
          </li>
          <li>
            <span class="dataGive__list__header">金額</span>
            <input class="dataGive__list__inputText"　type="text" name="Product" placeholder="商品金額">
          </li>
        </ul>
        <div class="dataGive__body__append">
          追加
        </div>
      </div>
    </div>

    <div class="dataProduct">
      <ol class="dataProduct__list">
        <li>
          <div class="dataProduct__image">
            Dummy
          </div>
          <div class="dataProduct__body">
            <div class="dataProduct__header">
              サンプル商品
            </div>
            <div class="dataProduct__text">
              こちらはサンプル商品です
            </div>
            <div class="dataProduct__price">
              &yen; 1,200
            </div>
            <div class="dataProduct__button">
              <div class="dataProduct__add">Add to Cart</div>
              <div class="dataProduct__delete">Delete</div>
            </div>
          </div>
        </li>
      </ol>
    </div>

    <div class="dataResult">
      <div class="dataResult__header">
        合計
      </div>
      <div class="dataResult__body">
        <div class="dataResult__text">
          <input type="text" id="dataResult" name="dataResult" readonly>
        </div>
        <div class="dataResult__init">
          初期化
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'componentTask4',
  data: function() {
    return {

    }
  },
  methods: {
  },
  computed: {
  },
}
</script>

<style lang="scss">
.dataGive {
  border: 1px solid #bbb;
  border-radius: 5px;
  margin-bottom: 30px;

  &__header {
    background-color: #f0f0f0;
    border-bottom: 1px solid #ddd;
    border-radius: 5px 5px 0 0;
    padding: 15px 20px;
  }

  &__body {
    display: flex;
    justify-content: space-between;
    align-items: end;
    padding: 25px 20px;

    &__list {
      flex-basis: 80%;

      > li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        line-height: 1.2;

        &:not(:last-of-type) { margin-bottom: 15px; }

        > span {
          display: inline-block;
          text-align: center;
          flex-basis: 15%;
        }

        > input {
          border: 1px solid #bbb;
          border-radius: 5px;
          flex-basis: 80%;
        }

        > span, > input { padding: 10px; }
      }
    }

    &__append {
      background-color: #f0f0f0;
      border: 1px solid #ddd;
      border-radius: 5px;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      padding: 10px 0;
      flex-basis: 15%;

      &:hover { background-color: #ccc; }
    }
  }
}

.dataProduct {
  margin-bottom: 30px;

  &__list {
    display: flex;
    flex-flow: row wrap; 

    > li {
      border: 1px solid #bbb;
      border-radius: 5px;
      margin-bottom: 30px;
      flex-basis: 31%;

      &:not(:nth-of-type(3n)) { margin-right: 3.5%; }
      &:not(:nth-last-of-type(n+3)) { margin-bottom: 0; }
    }
  }

  &__image {
    background-color: #666;
    color: #fff;
    text-align: center;
    padding: 50px 0;
  }

  &__body {
    padding: 15px 5px 5px 5px;
    > div:not(:last-of-type) { margin-bottom: 20px; }
  }

  &__header, &__text {
    line-height: 1.5;
    padding-left: 10px;
  }

  &__text { font-size: 75%; }

  &__header, &__price { font-weight: bold; }

  &__price {
    color: #df0000;
    text-align: center;
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
      flex-basis: 49%;

      &:hover { opacity: .5; }
    }
  }
  &__add { background-color: #fd7e00; }
  &__delete { background-color: #c00; }
}

.dataResult {
  display: flex;
  justify-content: space-between;
  align-items: center;

  &__header {
    display: inline-block;
    text-align: center;
    padding: 15px;
    width: 15%;
  }

  &__body {
    display: flex;
    justify-content: space-between;
    align-items: center;
    line-height: 1;
    width: 85%;
  }

  &__text {
    border: 1px solid #bbb;
    border-radius: 5px;
    padding: 10px;
    width: 80%;
  }

  &__init {
    background-color: #f0f0f0;
    border: 1px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
    text-align: center;
    padding: 15px;
    width: 15%;
  }

}

</style>