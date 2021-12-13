<template>
  <div class="funcCategoryGiveArea">
    <div class="categoryGive">
      <div class="categoryGive__header">データ追加</div>
      <div class="categoryGive__body">
        <ul class="categoryGive__body__list">
          <li>
            <span class="categoryGive__list__header">カテゴリ</span>
            <select class="categoryGive__list__inputOption" v-model="input.category">
              <option disabled value="" >〜 一覧からカテゴリーを選択 〜</option>
              <option v-for="category in categorys">{{ category }}</option>
            </select>
          </li>
          <li><span class="categoryGive__list__header">商品名</span><input class="categoryGive__list__inputText" type="text" v-model.trim="input.name" name="productName" placeholder="商品名称を入力" autocomplete="off"></li>
          <li><span class="categoryGive__list__header">詳細</span><input class="categoryGive__list__inputText" type="text" v-model.trim="input.detail" name="productDetail" placeholder="商品の詳細を入力" autocomplete="off"></li>
          <li><span class="categoryGive__list__header">金額</span><input class="categoryGive__list__inputText" type="number" v-model.trim="input.price" name="productPrice" placeholder="金額は1円以上の数値を入力" autocomplete="off"></li>
        </ul>
        <div class="categoryGive__body__append" @click="onClickDataAppend">追加</div>
      </div>
    </div>

    <div style="margin: 20px auto; width: 700px;">
      <div style="margin-bottom: 15px;">▼入力内容</div>
      <div style="background-color: #fafafa; font-size: 15px; padding: 20px;">{{ input }}</div>
    </div>
    <div style="margin: 20px auto; width: 700px;">
      <div style="margin-bottom: 15px;">▼追加内容</div>
      <div style="background-color: #fafafa; font-size: 15px; padding: 20px;">{{ products }}</div>
    </div>

    <div class="categoryProduct" v-if="flag.product">
      <div class="categoryProduct__tab">
        <div class="categoryProduct__tab__text" v-for="category in categorys">{{ category }}</div>
      </div>
      <ol class="categoryProduct__list">
        <task5Card v-for="(product, key) in products" :key="product.id" :product="product" @del="onClickListDelete(key)" />
      </ol>
    </div>
  </div>
</template>

<script>
import task5Card from "./task5component/card";

export default {
  name: 'componentTask5',
  components: {
    task5Card,
  },
  data: function() {
    return {
      flag: {
        product: false,
      },

      categorys: [
        'カテゴリー１',
        'カテゴリー２',
      ],

      input: {
        id: 0,
        category: '',
        name: '',
        detail: '',
        price: '',
        num: 1,
      },

      products: [],

      product: {},
    }
  },
  methods: {
    onClickDataAppend: function() {
      if(this.input.category !== '' && this.input.name !== '' && this.input.detail !== '' && this.input.price !== '') {
        if(this.input.price <= 0) {
          alert('金額は1円以上の数値を入力してください');
          this.input.price = '';

          return;
        }

        this.product = {
          id: this.input.id++,
          category: this.input.category,
          name: this.input.name,
          detail: this.input.detail,
          price: this.input.price,
          num: 1,
        }

        this.products.push(this.product);

        !this.flag.product ? this.flag.product = true : '';

        this.input.category = '';
        this.input.name = '';
        this.input.detail = '';
        this.input.price = '';
      }
    },

    onClickListDelete: function(num) {
      this.products.splice(num, 1);

      if(this.products.length === 0) {
        this.flag.product = false;
      }
    },
  },
}
</script>

<style lang="scss">
.categoryGive {
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
          text-align: left;
          flex-basis: 25%;
        }

        > select {
          appearance: auto;
          background-color: #fdd;
          cursor: pointer;
        }

        > select, > input {
          border: 1px solid #bbb;
          border-radius: 5px;
          flex-basis: 75%;
        }

        > span, > select, > input { padding: 10px; }
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

.categoryProduct {
  background-color: #f0f0f0;
  border: 1px solid #dfdfdf;
  border-radius: 5px;
  margin-bottom: 30px;
  padding-top: 25px;
  width: 800px !important;

  &__tab {
    display: flex;
    flex-flow: row wrap;
    padding: 0 20px;

    &__text {
      font-size: 14px;
      flex-basis: 15%;
      text-align: center;
      padding: 15px 10px;

      &.is_active {
        background-color: #fff;
        border-radius: 5px 5px 0 0;
        font-weight: bold;
      }
      &:not(.is_active) {
        cursor: pointer;
      }
    }
  }

  &__list {
    background-color: #fff;
    border-radius: 0 0 5px 5px;
    display: flex;
    flex-flow: row wrap;
    padding: 20px;
  }
}

.categoryTable {
  width: 900px !important;

  &__bulk {
    display: flex;
    justify-content: space-between;
    margin-left: auto;
    padding: 0 0 20px 15px;
    width: 40%;

    > div {
      border-width: 1px;
      border-style: solid;
      border-radius: 5px;
      color: #fff;
      cursor: pointer;
      text-align: center;
      transition: .4s;
      padding: 15px;
      flex-basis: 48%;
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

  table {
    &:not(:last-of-type) { margin-bottom: 20px; }
  }

  &__body {
    border: 1px solid #bbb;
    border-radius: 5px;
    text-align: center;
    table-layout: fixed;
    width: 100%;
  }

  &__header {
    background-color: #f0f0f0;
    border-bottom: 1px solid #bbb;

    &__chk { width: 80px; }

    th {
      text-align: center;
      vertical-align: middle;
      padding: 15px;

      &:not(:last-of-type) { border-right: 1px solid #bbb; }

      i {
        background-color: #fff;
        border-radius: 5px;
        border: 1px solid;
        cursor: pointer;
        font-size: 20px;
        transition: .4s;
        margin-left: 15px;
        padding: 0 5px;

        &:hover { background-color: #ffc9d2; }
      }
    }
  }
}
</style>