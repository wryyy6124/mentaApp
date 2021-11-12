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
            <input class="dataGive__list__inputText"　type="text" v-model.trim="p_name" name="productName" placeholder="商品名称を入力" autocomplete="off">
          </li>
          <li>
            <span class="dataGive__list__header">詳細</span>
            <input class="dataGive__list__inputText"　type="text" v-model.trim="p_detail" name="productDetail" placeholder="商品の詳細を入力" autocomplete="off">
          </li>
          <li>
            <span class="dataGive__list__header">金額</span>
            <input class="dataGive__list__inputText"　type="number" v-model.trim="p_price" name="productPrice" placeholder="金額は1円以上の数値を入力" autocomplete="off">
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
        <task4Card
          v-for="(product, key) in products" :key="product.id" :product="product"
          @add="onClickCalc(key)" @del="onClickListDelete(key)"
        />
      </ol>
    </div>

    <div class="dataCartTable" v-if="flag.cart">
      <table class="dataCartTable__body">
        <!-- テーブル見出し -->
        <tr class="dataCartTable__header">
          <th class="dataCartTable__header__chk">
            <input type="checkbox" v-model="flag.checkAll" @change="onAllChecked($event)">
            <!-- <br>検証用：{{ flag.checkAll }} -->
          </th>
          <th>商品名</th>
          <th>数量</th>
          <th>単価</th>
          <th>小計</th>
          <th>操作</th>
        </tr>

        <!-- カートに追加した商品の一覧と金額合計表示 -->
        <task4TableList
          v-for="(cart, key) in carts" :key="cart.id" :cart="cart"
          :checkAll="flag.checkAll" ref="child"
          @modalOpen="onClickModalOpen" @tableListDelete="onListDelete(key)"
        />
      </table>

      {{ flag.checkAll }}

      <!-- 合計金額 -->
      <task4TableCalc :carts="carts" />
    </div>

    <task4modalWindow
      v-if="flag.modal" :cartData="cartData"
      @modalClose="onClickModalClose" @changeNum="onChangeNum"
    />
  </div>
</template>

<script>
import task4Card from "./task4component/card";
import task4modalWindow from "./task4component/modalWindow";
import task4TableList from "./task4component/tableList";
import task4TableCalc from "./task4component/tableCalc";

export default {
  name: 'componentTask4',
  components: {
    task4Card, task4modalWindow,
    task4TableList, task4TableCalc,
  },
  data: function() {
    return {
      // 各セクションのフラグ管理
      flag: {
        product: false,
        cart: false,
        modal: false,
        checkAll: false,
        duplicate: false,
      },

      // 全商品データの格納（配列）
      products: [],

      // 追加用の入力データ各値のデータバインディング
      p_id: 0, // ID（リスト識別用）
      p_name: '', // 名前
      p_detail: '', // 詳細
      p_price: '', // 価格

      // 各商品情報を格納
      product: {},

      // 全商品データの格納（配列）
      carts: [],

      cartValue: {
        id: 0, // ID（リスト識別用）
        name: '', // 名前
        num: 1,
        price: '', // 価格
      },

      cartData: {
        id: 0,
        name: '', // 名前
        num: 1,
        price: '', // 価格
      },

    }
  },
  methods: {
    // 追加ボタン押下時に各入力項目を元にカード型リストを作成する
    onClickDataAppend: function() {
      // 各テキストフィールドに1字以上の入力がされているか
      if (this.p_name !== '' && this.p_detail !== '' && this.p_price !== '') {

        // 正しい価格が入力されているか
        if (this.p_price <= 0) {
          alert('金額は1円以上の数値を入力してください');
          this.p_price = "";

          return;
        }

        // 入力内容をオブジェクトへ格納
        this.product = {
          id: this.p_id++,
          name: this.p_name,
          detail: this.p_detail,
          num: 1,
          price: this.p_price,
        }

        // 配列にオブジェクト（各入力内容）を追加
        this.products.push(this.product);

        // 商品一覧が非表示であれば表示させる
        !this.flag.product ? this.flag.product = true : '';

        // データバインディング（各入力）部分の初期化
        this.p_name = '';
        this.p_detail = '';
        this.p_price = '';
      }
    },

    // AddtoCartボタン押下時
    onClickCalc: function(key) {
      // カートエリアが非表示であれば表示させる
      !this.flag.cart ? this.flag.cart = true : '';

      // 入力内容をオブジェクト（カート）へ格納
      this.cartValue = {
        id: this.products[key].id,
        name: this.products[key].name,
        num: this.products[key].num,
        price: this.products[key].price,
      }

      // 同一IDの商品（＝一度カートへ追加済み）があった場合、
      // 配列への追加自体は行わない代わりに注文数量へ可算(＋１)する
      for (let i = 0; i < this.carts.length; i++) {
        if (this.carts[i].id === this.cartValue.id) {
          this.carts[i].num += 1;
          this.duplicate = true;
        }
      }

      // 既にカートへ追加済みでない場合
      if (!this.duplicate) {
        this.carts.push(this.cartValue);
        return;
      }

      // 重複フラグ解除
      this.duplicate = false;
    },

    onAllChecked: function(e) {
      // this.$refs.child.onChangeAllCheck();
      console.log(e);


    },

    // Deleteボタン押下時：登録した各商品の一つを削除する
    onClickListDelete: function(num) {
      // カートに追加したアイテムが存在するか
      if(!(this.carts.length === 0)) {
        // カートへ追加済みのアイテムがあった場合、商品カード一覧からは消去出来ない旨、アラート発呼する
        for (let i = 0; i < this.products.length; i++) {
          if(this.products[num].id === this.carts[i].id) {
            alert(`カート一覧に追加中のアイテムの為、削除できません。カートに入った商品を削除してから再度お試しください。`);

            return;
          }
        }
      }
      this.products.splice(num, 1);

      // リストが空だったら合計金額部分を非活性化
      if (this.products.length === 0) {
        this.flag.product = false;
        this.flag.cart = false;
      }
    },

    // モーダルウィンドウ起動
    onClickModalOpen: function(id, name, num, price) {
      // ウィンドウ起動後の各項目を入力
      this.cartData.id = id;
      this.cartData.name = name;
      this.cartData.num = num;
      this.cartData.price = price;

      // ウィンドウ起動
      this.flag.modal = true;
    },

    // モーダルウィンドウ停止
    onClickModalClose: function() { this.flag.modal = false; },

    //  カートに追加した商品の数量を変更する
    onChangeNum: function(...args) {
      for (let i = 0; i < this.carts.length; i++) {        
        if (this.carts[i].id === args[0]) {
          this.carts[i].num = args[1];
        }
      }
    },

    // カートに追加した商品を削除する
    onListDelete: function(num) {
      // 該当の商品をカート一覧から削除する
      this.carts.splice(num, 1);

      // カート一覧が0件になったら非表示とする
      this.carts.length === 0 ? this.flag.cart = false : '';
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
    table-layout: fixed;
    &:not(:last-of-type) { margin-bottom: 20px; }
  }

  &__header {
    &__chk {
      width: 80px;
    }
  }
}

</style>