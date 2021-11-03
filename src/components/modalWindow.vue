<template>
  <div class="modalWindow">
    <div class="modalWindow__body">
      <div class="modalWindow__body__contents">
        <div class="modalWindow__body__closeBtn" @click="$emit('close')">
          Close<i class="fas fa-times"></i>
        </div>
        あああ
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'componentTask1',
  data: function() {
    return {
    }
  },
  methods: {
  },
}
</script>

<style lang="scss">
.modalWindow {
  animation-name: displayToggleAnime;
  animation-duration: 1s;

  &__body {
    background-color: rgba(0, 0, 0, .5);
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    overflow: auto;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;

    &__contents {
      background-color: #f4f4f4;
      box-shadow: 0 5px 8px 0 rgba(0, 0, 0, .2),
                  0 7px 20px 0 rgba(0, 0, 0, .2);
      border-radius: 10px;
      position: relative;
      width: 30%;
      height: 30%;
    }

    &__closeBtn {
      color: #fff;
      cursor: pointer;
      font-size: 25px;
      display: flex;
      align-items: center;
      transition: .7s;
      position: absolute;
      right: 0;
      top: -50px;

      > i { 
        font-size: 1.5em;
        margin-left: 10px;
      }

      &:hover { color: #e4ed63; }
    }
  }
}
</style>
