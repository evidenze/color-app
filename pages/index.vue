<template>
  <div>
    <div class="container">
      <h4>Filters</h4>
      <p class="text-primary font-weight-bold">Shapes</p>
      <button @click="filterShape('oval')" class="btn rounded-pill btn-outline-secondary">Oval</button>
      <button @click="filterShape('round')" class="btn rounded-pill btn-outline-secondary">Round</button>
      <button @click="filterShape('triangle')" class="btn rounded-pill btn-outline-secondary">Triangle</button>
      <button @click="filterShape('square')" class="btn rounded-pill btn-outline-secondary">Square</button>
      <button @click="filterShape('rectangle')" class="btn rounded-pill btn-outline-secondary mt-2 mt-lg-0">Rectangle</button>
      
      <p class="text-primary font-weight-bold pt-4">Colors</p>
      <div class="d-flex">
        <div @click="filterColor('blue')" class="blue-color"></div>
        <div @click="filterColor('red')" class="red-color"></div>
        <div @click="filterColor('green')" class="green-color"></div>
        <div @click="filterColor('yellow')" class="yellow-color"></div>
        <div @click="filterColor('cyan')" class="cyan-color"></div>
        <div @click="filterColor('gray')" class="gray-color"></div>
      </div>

      <h5 class="font-weight-bold pt-4">({{ items.length }})</h5>
      <div class="row mt-3">
        <div v-for="data in items" :key="data.id" class="col-md-4 mb-3">
          <div class="card shadow bg-white text-center p-5 border-0">
            <div v-if="data.shape == 'round'" class="circle mr-auto ml-auto" :style="{background: data.color}">
            </div>
            <div v-if="data.shape == 'oval'" class="oval mr-auto ml-auto" :style="{background: data.color}">
            </div>
            <div v-if="data.shape == 'triangle'" class="triangle mr-auto ml-auto" :style="{background: data.color}">
            </div>
            <div v-if="data.shape == 'rectangle'" class="rectangle mr-auto ml-auto" :style="{background: data.color}">
            </div>
            <div v-if="data.shape == 'square'" class="square mr-auto ml-auto" :style="{background: data.color}">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import data from "~/static/items.json";


export default {
  name: 'IndexPage',

  data() {
    return {
      items: data.items,
      activeShape: '',
      activeColor: '',
    }
  },

  methods: {
    filterShape(shape) {
      let activeColor = this.activeColor;
      this.items = [];
      var newArray = data.items.filter(function (el) {
        if (activeColor == '') {
          return el.shape == shape;
        } else {
          return el.shape == shape && el.color == activeColor;
        }
      });

      this.items = newArray;
      this.activeShape = shape;
    },

    filterColor(color) {
      let activeShape = this.activeShape;
      this.items = [];
      var newArray = data.items.filter(function (el) {
        if (activeShape == '') {
          return el.color == color
        } else {
          return el.color == color && el.shape == activeShape;
        }
      });

      this.items = newArray;
      this.activeColor = color;
    }
  }
}
</script>



<style scoped>

  .circle {
    width: 100px;
    height: 100px;
    border-radius: 50%;
  }

  .oval {
    width: 150px;
    height: 100px;
    border-radius: 50%;
  }

  .triangle {
    width: 100px;
    height: 100px;
    clip-path: polygon(0% 100%, 50% 0%, 100% 100%);
  }

  .rectangle {
    width: 200px;
    height: 100px;
  }

  .square {
    width: 100px;
    height: 100px;
  }


  .blue-color{
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background: blue;
  }

  .red-color{
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background: red;
  }

  .yellow-color{
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background: yellow;
  }

  .cyan-color{
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background: cyan;
  }

  .gray-color{
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background: gray;
  }

</style>
