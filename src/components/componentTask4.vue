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

    <div class="dataProduct">
      <ol class="dataProduct__list">
        <li v-for="(item, index) in lists" :key="index">
          <div class="dataProduct__image">Dummy</div>
          <div class="dataProduct__body">
            <div class="dataProduct__name">{{ item.name }}</div>
            <div class="dataProduct__detail">{{ item.detail }}</div>
            <div class="dataProduct__price"><span>&yen;</span>{{ item.price }}</div>
            <div class="dataProduct__button">
              <div class="dataProduct__add">Add to Cart</div>
              <div class="dataProduct__delete" @click="onClickListDelete(index)">Delete</div>
            </div>
          </div>
        </li>
      </ol>
    </div>

    <div class="dataResult" v-if="result.display">
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
  props: {
    products : {
      // 名前
      name: [String, Number],
      // 詳細
      detail: [String, Number],
      // 価格
      price: Number,
    }
  },
  data: function() {
    return {
      // 親コンポーネント（App.vue）から渡ってきた配列を格納
      lists: this.products,

      //各商品情報を格納する
      product: {},

      // 名前
      name: '',
      // 詳細
      detail: '',
      // 価格
      price: '',

      // カートに入れた商品の合計金額
      result: {
        total: 0,
        display: false,
      }
    }
  },
  methods: {
    onClickDataAppend: function() {
      // 各テキストフィールドに1字以上の入力がされているか
      if (this.name !== '' && this.detail !== '' && this.price !== '') {
        if (this.price <= 0) {
          alert('金額は1円以上の数値を入力してください');
          return;
        }

        // 入力内容をオブジェクトへ格納
        this.product = {
          name: this.name,
          detail: this.detail,
          price: Number(this.price).toLocaleString(),
        }

        // 配列にオブジェクト（各入力内容）を追加
        this.lists.push(this.product);

        // 合計金額部分を活性化
        this.result.display = true;

        // 入力内容の初期化
        this.name = '';
        this.detail = '';
        this.price = '';
      }
    },

    onClickListDelete: function(num) {
      this.lists.splice(num, 1);
      console.log(this.lists.length);

      if (this.lists.length === 0) {
        // リストが空だったら合計金額部分を非活性化
        this.result.display = false;
      }
    },
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
      &:last-of-type { margin-right: 0; }

      &:not(:nth-of-type(-n+3)) { margin-bottom: 0; }
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

    > span {
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