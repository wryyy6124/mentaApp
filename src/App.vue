<template>
  <div id="app">
    <p v-if="testFlg">Flg is OK</p>
    <p v-if="!testFlg">Flg is NG</p>
    <p>{{ msg }}</p>
    <button v-on:click="clickTest">切り替え</button>
  </div>
</template>

<script>
export default {
  name: 'Apple',
  data: function() {
    return {
      msg: 'Test First Message',
      testFlg: false,
    }
  },
  methods: {
    clickTest: function() {
      if(this.testFlg) {
        this.msg = 'This is NG Msg';
        this.testFlg = false;
      } else {
        this.msg = 'This is OK Msg';
        this.testFlg = true;
      }
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
