<template>
<div>
  <div class="board">

    <canvas ref="randomBoardCanvasRef" @click="boardClick" :width="size.w" :height="size.h" tabindex='0'
            style="border:1px solid #000000;"
    ></canvas>

  </div>

  <div class="controls">
      <button @click="run">Run</button>
  </div>
</div>
</template>

<script>

import { reactive } from 'vue'
import {interval} from "rxjs";
import {take} from "rxjs";

export default {

  setup() {

    let size = reactive({
       w: 1000,
       h: 800
    });

    let card = reactive([]);

    for(let i=0; i < size.w / 5; i++) {
      card[i] = [];
      for(let j=0; j < size.h / 5; j++) {
        if (Math.random() * 100 > 75) {
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

    for(let i=0; i < this.size.w / 5; i++) {
      for(let j=0; j < this.size.h / 5; j++) {
        if (this.card[i][j]) {
          randomBoardCanvasContext.fillStyle = this.card[i][j].color;
          randomBoardCanvasContext.fillRect(i * 5, j * 5, 5, 5);
        }
      }
    }

  },

  methods: {

    run() {

      let randomBoardCanvasContext = this.$refs['randomBoardCanvasRef'].getContext('2d');

        interval().pipe().subscribe(res => {

          let i = Math.floor(Math.random() * (this.size.w / 5))
          let j = Math.floor(Math.random() * (this.size.h / 5))

          if (this.card[i][j]) {

            if (this.card[i][j].color === 'white') {
                if (Math.random() * 100 > 5) {
                  this.card[i][j].color = 'green';
                } else {
                  this.card[i][j].color = 'red';
                }
            }

            randomBoardCanvasContext.fillStyle = this.card[i][j].color;
            randomBoardCanvasContext.fillRect(i * 5, j * 5, 5, 5);

            // console.log('this.card[i][j]:', this.card[i][j]);
          }
        })
    },

    boardClick(event) {

      if (event.offsetX && event.offsetY) {

        let randomBoardCanvasContext = this.$refs['randomBoardCanvasRef'].getContext('2d');

        let i = Math.floor(event.offsetX / 5);
        let j = Math.floor(event.offsetY / 5);

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
          randomBoardCanvasContext.fillRect(i * 5, j * 5, 5, 5);

        }
      }
    }
  }

}

</script>

<style scoped>

</style>