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
            <input class="dataGive__list__inputText"　type="text" v-model.trim="name" name="productName" placeholder="商品名称を入力" autocomplete="off">
          </li>
          <li>
            <span class="dataGive__list__header">詳細</span>
            <input class="dataGive__list__inputText"　type="text" v-model.trim="detail" name="productDetail" placeholder="商品の詳細を入力" autocomplete="off">
          </li>
          <li>
            <span class="dataGive__list__header">金額</span>
            <input class="dataGive__list__inputText"　type="number" v-model.trim="price" name="productPrice" placeholder="金額は1円以上の数値を入力" autocomplete="off">
          </li>
        </ul>
        <div class="dataGive__body__append" @click="onClickDataAppend">
          追加
        </div>
      </div>
    </div>

    <div class="dataProduct" v-if="flag.product">
      <ol class="dataProduct__list">
        <!-- データ追加した商品一覧を追加した数分カード表示する -->
        <task4Card v-for="(product, key) in products" :key="product.id" :product="product" @add="onClickCalc(key)" @del="onClickListDelete(key)" />
      </ol>
    </div>

    <div class="dataCartTable" v-if="flag.cart">
      <!-- カートに追加した商品の一覧と金額合計表示 -->
      <task4TableList @modal="$emit('modal')" />
      <task4TableCalc />
    </div>

  </div>
</template>

<script>
import task4Card from "./task4component/card";
import task4TableList from "./task4component/tableList";
import task4TableCalc from "./task4component/tableCalc";

export default {
  name: 'componentTask4',
  components: {
    task4Card,
    task4TableList,
    task4TableCalc,
  },
  data: function() {
    return {
      
      // 全商品データの格納（配列）
      products: [],

      // 各値のデータバインディング
      id: 1, // ID（リスト識別用）
      name: '', // 名前
      detail: '', // 詳細
      price: '', // 価格

      // 各商品情報を格納
      product: {},

      // フラグ管理
      flag: {
        product: false,
        cart: false,
      },

    }
  },
  methods: {
    // 追加ボタン押下時：各入力項目を元にカード型リストを作成する
    onClickDataAppend: function() {
      // 各テキストフィールドに1字以上の入力がされているか
      if (this.name !== '' && this.detail !== '' && this.price !== '') {
        if (this.price <= 0) {
          alert('金額は1円以上の数値を入力してください');
          return;
        }

        // 入力内容をオブジェクトへ格納
        this.product = {
          id: this.id++,
          name: this.name,
          detail: this.detail,
          price: this.price,
        }

        // 配列にオブジェクト（各入力内容）を追加
        this.products.push(this.product);

        // 追加データが１件でもあれば活性化させる
        this.flag.product = true;

        // 入力内容の初期化
        this.name = '';
        this.detail = '';
        this.price = '';
      }
    },

    // AddtoCartボタン押下時
    onClickCalc: function(num) {
      this.flag.cart = true;
    },

    // Deleteボタン押下時：登録した各商品の一つを削除する
    onClickListDelete: function(num) {
      this.products.splice(num, 1);

      if (this.products.length === 0) {
        // リストが空だったら合計金額部分を非活性化
        this.flag.product = true;
        this.flag.cart = true;
      }
    },
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
  }
}

.dataCartTable {
  width: 900px !important;

  table {
    border: 1px solid #bbb;
    text-align: center;
    &:not(:last-of-type) { margin-bottom: 20px; }
  }
}

</style>