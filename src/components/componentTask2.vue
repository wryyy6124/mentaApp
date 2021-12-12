<template>
  <div class="funclistArea">
    <div class="listCreate">
      <button class="listCreate__button" :class="{ is_deactive: btnChange }" @click="listCreate(listLength)">リスト作成</button>
      <button class="listCreate__button" @click="listLengthInit">初期化</button>
    </div>
    <div class="listSelect">
      <div class="listSelect__header">リスト数</div>
      <div class="listSelect__number"><input type="number" v-model="listLength" @click="numberJudgement(listLength)"></div>
    </div>

    <div :class="{ errMessage: errFlag }" v-if="errFlag">マイナスは選べません</div>

    <div class="createResult">
      <div class="createResult__header">リスト</div>
      <ul class="createResult__list">
        <li v-for="(list, index) in lists" :key="list">サンプル {{ index + 1 }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'componentTask2',
  data: function() {
    return {
      listLength: 0,
      lists: {},
      errFlag: false,
      btnChange: false,
    }
  },
  methods: {
    listCreate: function(len) { this.lists = new Array(Number(len)); },
    listLengthInit: function() {
      this.listLength = 0;
      this.lists = {},
      this.errFlag = false;
      this.btnChange = false;
    },
    numberJudgement: function(num) {
      if(Number(num) < 0) {
        this.errFlag = true;
        this.btnChange = true;
        return true;
      }
      this.errFlag = false;
      this.btnChange = false;
      return false;
    }, 
  },
}
</script>

<style lang="scss">
.funclistArea {
  > div {
    &:not(:last-of-type) { margin-bottom: 25px; }
  }
}

.listCreate {
  display: flex;
  justify-content: end;
  align-items: center;

  &__button {
    background-color: #ddd;
    border: 1px solid #666;
    border-radius: 5px;
    color: #232323;
    cursor: pointer;
    text-align: center;
    transition: .4s;
    padding: 10px 0;
    flex-basis: 31%;

    &:not(:first-of-type) { margin-left: 3.5%; }

    &.is_deactive {
      background-color: #ddd;
      border: 1px solid #bbb;
      color: #bbb;
      cursor: default;
      font-weight: normal;
      opacity: .8;
      pointer-events: none;
    }
  }
}

.listSelect {
  display: flex;
  justify-content: end;
  align-items: center;

  > div {
    &:not(:first-of-type) { margin-left: 3.5%; }
  }

  &__header {
    text-align: right;
    width: 30%;
  }

  &__number {
    border: 1px solid #666;
    text-align: center;
    width: 15%;

    > input {
      border: none;
      font-size: 20px;
      text-align: center;
      padding: 10px 0;
      width: 100%;
    }
  }
}

.createResult {
  border: 1px solid #bbb;
  border-radius: 5px;
  text-align: center;

  &__header {
    background-color: #f0f0f0;
    border-radius: 5px 5px 0 0;
    padding: 10px 0;
    width: 100%;
  }

  &__list {
    list-style: none;
    margin: 0;
    padding: 0;

    > li {
      border-top: 1px solid #aaa;
      padding: 10px 0;
    }
  }
}

.errMessage {
  background-color: #f1d9d9;
  border: 1px solid #500;
  border-radius: 5px;
  color: #500;
  transition: .4s;
  text-align: center;
  padding: 10px 0;
}
</style>