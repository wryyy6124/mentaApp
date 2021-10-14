<template>
  <div id="app">
    <div class="selectArea">
      <button class="selectArea__button" v-on:click="btnClickCalc">プラス</button>
      <button class="selectArea__button" v-on:click="btnClickCalc">マイナス</button>
      <button class="selectArea__button" v-on:click="btnClickCalc">初期化</button>
    </div>
    <div class="resultArea">
      <dt class="resultArea__header">結果</dt>
      <dd class="resultArea__body">{{ result }}</dd>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function() {
    return {
      result: 0,
    }
  },
  methods: {
    btnClickCalc: function(e) {
      // 押下したボタンのテキストで計算方法を判定・処理を実行する
      e.target.innerText === 'プラス' ? this.result++ : '';
      e.target.innerText === 'マイナス' ? this.result-- : '';
      e.target.innerText === '初期化' ? this.result = 0 : '';
    },
  }
}
</script>

<style>
#app {
  color: #2c3e50;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

.selectArea,
.resultArea {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 auto;
  width: 500px;
}

.selectArea { margin-bottom: 30px; }

.selectArea__button {
  cursor: pointer;
  padding: 10px 0;
  width: 31%;
}

.resultArea { font-size: 20px; }

.resultArea__header,
.resultArea__body { width: 49%; }

.resultArea__header { text-align: right; }

.resultArea__body {
  border: 1px solid #666;
  text-align: center;
  padding: 10px;
}

</style>
