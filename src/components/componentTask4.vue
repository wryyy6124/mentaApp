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
      <!-- 一括操作 -->
      <div class="dataCartTable__bulk" v-if="flag.bulk">
        <div class="dataCartTable__bulk__edit" @click="onClickBulkEdit">一括編集</div>
        <div class="dataCartTable__bulk__delete" @click="onClickBulkDel">一括削除</div>
      </div>

      {{ carts }}

      <table class="dataCartTable__body">
        <!-- テーブル見出し -->
        <tr class="dataCartTable__header">
          <th class="dataCartTable__header__chk">
            <input type="checkbox" v-model="flag.checkAll" @change="onAllChecked">
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
          :checkAll="flag.checkAll" @chkboxSwitch="chkAllSwitch"
          @modalOpen="onClickModalOpen" @tableListDelete="onListDelete(key)"
        />
      </table>

      <!-- 合計金額 -->
      <task4TableCalc :carts="carts" />
    </div>

    <task4modalWindow
      v-if="flag.modal" :cartData="cartData" :cartsData="carts"
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
        bulk: false,
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
          price: this.p_price,
          num: 1,
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
        price: this.products[key].price,
        num: this.products[key].num,
        chk: false,
      }

      // 同一IDの商品（＝一度カートへ追加済み）があった場合、
      // 配列への追加自体は行わない代わりに注文数量へ可算(＋１)する
      for (let i=0; i<this.carts.length; i++) {
        if (this.carts[i].id === this.cartValue.id) {
          this.carts[i].num += 1;
          this.duplicate = true;
        }
      }

      // カート一覧へ追加済みでない場合
      if (!this.duplicate) {
        this.carts.push(this.cartValue);
        return;
      }

      // 重複フラグ解除
      this.duplicate = false;
    },

    onAllChecked: function() {
      // 全チェック欄のチェックが付いている
      if(this.flag.checkAll) {
        for(let i=0; i<this.carts.length; i++) { this.carts[i].chk = true; }  // 各ボックスのチェックを入れる
        this.flag.bulk = true;  // 一括操作の項目を表示させる

        return; // 以降は実施する必要がないので処理を抜ける
      }

      // 上記if文をスルーした為、全チェック欄のチェックが存在しなかった
      for(let i=0; i<this.carts.length; i++) { this.carts[i].chk = false; } // 各ボックスのチェックを外す
      this.flag.bulk = false; // 一括操作の項目を非表示とする
    },

    chkAllSwitch: function() {
      // カート一覧の各チェックボックスが「１つでもチェック済み」であれば、一括操作の項目を表示させる
      for(let i=0; i<this.carts.length; i++) {
        if(this.carts[i].chk) {
          this.flag.bulk = true;
          break;  // チェック済みの項目が見つかったので、ループを抜ける
        }

        this.flag.bulk = false; // チェック済みの項目が存在しなかったので、一括操作の項目を非表示とする
      }

      // カート一覧の各チェックボックスが「１つでも外れたら」全チェックボックスもチェックを外す
      for(let i=0; i<this.carts.length; i++) {
        if(!this.carts[i].chk) {
          this.flag.checkAll = false;
          return; // チェックが付いていない箇所をこの時点で発見できた。以降は必要ないので処理終了
        }
      }

      // 上記ループを何事もなく抜けたということは、全てチェックがついていたということなので全チェックを付与する
      this.flag.checkAll = true;

      // 一括操作の項目を表示させる
      this.flag.bulk = true;
    },

    // Deleteボタン押下時：登録した各商品の一つを削除する
    onClickListDelete: function(num) {
      // カートへ追加済みのアイテムがあった場合、商品カード一覧からは消去出来ない旨、アラート発呼する
      for(let i=0; i<this.carts.length; i++) {
        if(this.products[num].id == this.carts[i].id) {
          alert(`カート一覧に追加中のアイテムの為、削除できません。カートに入った商品を削除後に再度お試しください。`);
          return;
        }
      }

      // カート一覧が存在しなかったので商品カード一覧から該当のオブジェクトを消去
      this.products.splice(num, 1);

      // 商品カードが0件
      if (this.products.length === 0) {
        this.flag.product = false;
        this.flag.cart = false;
        this.flag.checkAll = false;
        this.carts = [];
      }

      // カートリストが0件 
      if (this.carts.length === 0) {
        this.flag.cart = false;
        this.flag.checkAll = false;
      }
    },

    // モーダルウィンドウ起動
    onClickModalOpen: function(data) {
      // ウィンドウ起動後の各項目を入力
      this.cartData.id = data.id;
      this.cartData.name = data.name;
      this.cartData.num = data.num;
      this.cartData.price = data.price;

      // ウィンドウ起動
      this.flag.modal = true;
    },

    // モーダルウィンドウ停止
    onClickModalClose: function() { this.flag.modal = false; },

    //  カートに追加した商品の数量を変更する
    onChangeNum: function(...args) {
      for (let i=0; i<this.carts.length; i++) {        
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

    // カートに追加した商品の中でチェック済みの項目を一括編集
    onClickBulkEdit: function() {

      for(let i=0; i<this.carts.length; i++) {
        if(this.carts[i].chk) {
          // ウィンドウ起動後の各項目を入力
          this.cartData.id = this.carts[i].id;
          this.cartData.name = this.carts[i].name;
          this.cartData.num = this.carts[i].num;
          this.cartData.price = this.carts[i].price;

          // ウィンドウ起動
          this.onClickModalOpen(this.cartData);
          console.log('if構文の中です');
        }
        console.log('ループの中です');
      }
      console.log('ループの外です');

    },

    // カートに追加した商品の中でチェック済みの項目を一括削除
    onClickBulkDel: function() {
      // 削除するかしないか確認ダイアログが出現
      const response = confirm('この一括操作は取り消しできません。本当に削除しますか？');

      console.log(this.carts);

      if(response) {
        for(let i=0; i<this.carts.length; i++) {
          if(this.carts[i].chk) {
            this.carts.splice(i, 1);
          }
        }

        alert(`チェックした項目の一括削除が完了しました`);
        return;
      }

      alert(`一括削除をキャンセルしました`);
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