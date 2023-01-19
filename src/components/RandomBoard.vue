<template>

  <div class="board">

    <canvas ref="randomBoardCanvasRef" @click="boardClick" :width="size.w" :height="size.h" tabindex='0'
            style="border:1px solid #000000;"
    ></canvas>

  </div>

</template>

<script>

import { reactive } from 'vue'

export default {

  setup() {

    let size = reactive({
       w: 1000,
       h: 800
    });

    let card = reactive([]);

    for(let i=0; i < size.w / 10; i++) {
      card[i] = [];
      for(let j=0; j < size.h / 10; j++) {
        if (Math.random() * 100 > 97) {
          if (Math.random() * 100 > 65) {
            card[i][j] = {color: 'green'};
          } else {
            card[i][j] = {color: 'red'};
          }
        } else {
          card[i][j] = { color: 'white' };
        }
      }
    }

    return {
      size,
      card
    }
  },

  mounted() {

    console.log('step mounted 2');

    let randomBoardCanvasContext = this.$refs['randomBoardCanvasRef'].getContext('2d');

    for(let i=0; i < this.size.w / 10; i++) {
      for(let j=0; j < this.size.h / 10; j++) {
        if (this.card[i][j]) {
          randomBoardCanvasContext.fillStyle = this.card[i][j].color;
          randomBoardCanvasContext.fillRect(i * 10, j * 10, 10, 10);
        }
      }
    }

  },

  methods: {
    boardClick(event) {

      if (event.offsetX && event.offsetY) {

        let randomBoardCanvasContext = this.$refs['randomBoardCanvasRef'].getContext('2d');

        let i = Math.floor(event.offsetX / 10);
        let j = Math.floor(event.offsetY / 10);

        console.log(i, j, event.offsetX, event.offsetY);

        if (this.card[i][j]) {

          if (this.card[i][j].color === 'green' || this.card[i][j].color === 'red') {
            this.card[i][j].color = 'white';
          } else {
            if (Math.random() * 100 > 40) {
              this.card[i][j].color = 'green';
            } else {
              this.card[i][j].color = 'red';
            }
          }

          randomBoardCanvasContext.fillStyle = this.card[i][j].color;
          randomBoardCanvasContext.fillRect(i * 10, j * 10, 10, 10);

        }
      }
    }
  }

}

</script>

<style scoped>

</style>