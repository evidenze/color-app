<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container">
      <a class="navbar-brand font-weight-bold" href="#">SHAPES</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav"
        aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item active">
            <nuxt-link @click.native="logout" class="nav-link text-danger" to="#">Logout</nuxt-link>
          </li>
        </ul>
      </div>
      </div>
    </nav>

    <div class="container mt-4">
      <h4>Filters</h4>
      <p class="text-primary font-weight-bold">Shapes</p>
      
      <ul class="ks-cboxtags">
        <li><input @change="changeShape($event)" value="oval" type="checkbox" id="oval" checked><label for="oval">Oval</label></li>
        <li><input @change="changeShape($event)" value="round" type="checkbox" id="round" checked><label for="round">Round</label></li>
        <li><input @change="changeShape($event)" value="triangle" type="checkbox" id="triangle" checked><label for="triangle">Triangle</label></li>
        <li><input @change="changeShape($event)" value="square" type="checkbox" id="square" checked><label for="square">Square</label></li>
        <li><input @change="changeShape($event)" value="rectangle" type="checkbox" id="rectangle" checked><label for="rectangle">Rectangle</label></li>
      </ul>

      <p class="text-primary font-weight-bold pt-4">Colors</p>
      <div class="mt-3">
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="blue" checked>
        <span class="checkmark checkmark-blue"></span>
      </label>
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="red" checked>
        <span class="checkmark checkmark-red"></span>
      </label>
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="green" checked>
        <span class="checkmark checkmark-green"></span>
      </label>
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="yellow" checked>
        <span class="checkmark checkmark-yellow"></span>
      </label>
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="cyan" checked>
        <span class="checkmark checkmark-cyan"></span>
      </label>
      <label class="contain">
        <input @change="filterColor($event)" type="checkbox" value="gray" checked>
        <span class="checkmark checkmark-gray"></span>
      </label>
      </div>

      <h5 class="font-weight-bold pt-4">{{ title }}: ({{ items.length }})</h5>
      <div class="row mt-3">
        <div v-for="data in items" :key="data.id" class="col-md-4 mb-3">
          <div class="card shadow bg-white text-center p-4 border-0">
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
  middleware: 'auth',

  data() {
    return {
      items: data.items,
      activeShape: ['oval', 'round', 'triangle', 'square', 'rectangle'],
      activeColors: ['red', 'green', 'yellow', 'cyan', 'gray', 'blue'],
      title: 'All items'
    }
  },

  methods: {
    async logout() {
      await this.$auth.logout();
      this.$router.push('/login');
    },

    changeShape(e) {
      if (e.target.checked) {
        var newArray = data.items.filter(data => data.shape == e.target.value && this.activeColors.includes(data.color));
        if (this.items.some(data => data.shape == e.target.value)) {
          this.items = this.items;
        } else {
          this.items = this.items.concat(newArray);
          this.activeShape.push(e.target.value);

          if (this.activeColors.length == 6 && this.activeShape.length == 5) {
            this.title = 'All items'
          } else if (this.activeShape.length == 6 && this.activeColors.length == 1) {
            this.title = 'All ' + this.activeColors[0] + ' items';
          } else if (this.activeShape.length > 1 && this.activeColors.length == 1) {
            this.title = 'Multiple ' + this.activeColors[0] + ' items';
          } else if (this.activeColors.length > 1 && this.activeShape.length == 1) {
            this.title = 'Multiple ' + this.activeShape[0] + ' items';
          } else if (this.activeColors.length == 1 && this.activeShape.length == 1) {
            this.title = this.activeColors[0] + ' ' + this.activeShape[0] + ' item'
          } else if (this.activeColors.length == 6 && this.activeShape.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length == 5 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length > 1 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          }
        }
      } else {
        var newArray = this.items.filter(data => data.shape !== e.target.value);
        if (!newArray.length) {
          this.items = data.items;
          this.activeShape = ['oval', 'round', 'triangle', 'square', 'rectangle'];
          this.activeColors.length == 6 ? this.title = 'All items' : this.title = 'Multiple items';
        } else {
          this.items = newArray;
          this.activeShape.splice(this.activeShape.indexOf(e.target.value), 1);

          if (this.activeColors.length == 6 && this.activeShape.length == 5) {
            this.title = 'All items'
          } else if (this.activeShape.length == 6 && this.activeColors.length == 1) {
            this.title = 'All ' + this.activeColors[0] + ' items';
          } else if (this.activeShape.length > 1 && this.activeColors.length == 1) {
            this.title = 'Multiple ' + this.activeColors[0] + ' items';
          } else if (this.activeColors.length > 1 && this.activeShape.length == 1) {
            this.title = 'Multiple ' + this.activeShape[0] + ' items';
          } else if (this.activeColors.length == 1 && this.activeShape.length == 1) {
            this.title = this.activeColors[0] + ' ' + this.activeShape[0] + ' item';
          } else if (this.activeColors.length == 6 && this.activeShape.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length == 5 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length > 1 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          }
        }
      }
    },

    filterColor(e) {
      if (e.target.checked) {
        var newArray = data.items.filter(data => data.color == e.target.value && this.activeShape.includes(data.shape));

        if (this.items.some(data => data.color == e.target.value)) {
          this.items = this.items;
        } else {
          this.activeColors.push(e.target.value);
          this.items = this.items.concat(newArray);

          if (this.activeColors.length == 6 && this.activeShape.length == 5) {
            this.title = 'All items'
          } else if (this.activeShape.length == 6 && this.activeColors.length == 1) {
            this.title = 'All ' + this.activeColors[0] + ' items';
          } else if (this.activeShape.length > 1 && this.activeColors.length == 1) {
            this.title = 'Multiple ' + this.activeColors[0] + ' items';
          } else if (this.activeColors.length > 1 && this.activeShape.length == 1) {
            this.title = 'Multiple ' + this.activeShape[0] + ' items';
          } else if (this.activeColors.length == 1 && this.activeShape.length == 1) {
            this.title = this.activeColors[0] + ' ' + this.activeShape[0] + ' item';
          } else if (this.activeColors.length == 6 && this.activeShape.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length == 5 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length > 1 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          }
        }
      } else {
        var arr = this.items.filter(function (data) {
          return data.color !== e.target.value && this.indexOf(data) < 0;
        }, this.activeShape);

        if (!arr.length) {
          this.items = data.items;
          this.activeColors = ['red', 'green', 'yellow', 'cyan', 'gray', 'blue'];
          this.activeShape.length == 6 ? this.title = 'All items' : this.title = 'Multiple items';
        } else {
          this.activeColors.splice(this.activeColors.indexOf(e.target.value), 1);
          this.items = arr;

          if (this.activeColors.length == 6 && this.activeShape.length == 5) {
            this.title = 'All items'
          } else if (this.activeShape.length == 6 && this.activeColors.length == 1) {
            this.title = 'All ' + this.activeColors[0] + ' items';
          } else if (this.activeShape.length > 1 && this.activeColors.length == 1) {
            this.title = 'Multiple ' + this.activeColors[0] + ' items';
          } else if (this.activeColors.length > 1 && this.activeShape.length == 1) {
            this.title = 'Multiple ' + this.activeShape[0] + ' items';
          } else if (this.activeColors.length == 1 && this.activeShape.length == 1) {
            this.title = this.activeColors[0] + ' ' + this.activeShape[0] + ' item';
          } else if (this.activeColors.length == 6 && this.activeShape.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length == 5 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          } else if (this.activeShape.length > 1 && this.activeColors.length > 1) {
            this.title = 'Multiple items'
          }
        }
      }
    },
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

    ul.ks-cboxtags {
      list-style: none;
      padding-left: 0;
    }
  
    ul.ks-cboxtags li {
      display: inline;
    }
  
    ul.ks-cboxtags li label {
      display: inline-block;
      background-color: rgba(255, 255, 255, .9);
      border: 2px solid rgba(139, 139, 139, .3);
      color: gray;
      border-radius: 25px;
      white-space: nowrap;
      margin: 3px 0px;
      -webkit-touch-callout: none;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
      transition: all .2s;
    }
  
    ul.ks-cboxtags li label {
      padding: 5px 12px;
      cursor: pointer;
    }
  
    ul.ks-cboxtags li input[type="checkbox"]:checked+label {
      border: 2px solid #eaeaea;
      background-color: #eaeaea;
      color: #000;
      transition: all .2s;
    }
  
    ul.ks-cboxtags li input[type="checkbox"] {
      display: absolute;
    }
  
    ul.ks-cboxtags li input[type="checkbox"] {
      position: absolute;
      opacity: 0;
    }
  
    ul.ks-cboxtags li input[type="checkbox"]:focus+label {
      border: 2px solid #eaeaea;
    }

    /* Customize the label (the container) */
.contain {
  display: inline;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.contain input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark-blue {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: blue;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked ~ .checkmark-blue {
  background-color: blue;
}

.checkmark-red {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: red;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked~.checkmark-red {
  background-color: red;
}

.checkmark-yellow {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: yellow;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked~.checkmark-yellow {
  background-color: yellow;
}

.checkmark-green {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: green;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked~.checkmark-green {
  background-color: green;
}

.checkmark-cyan {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: cyan;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked~.checkmark-cyan {
  background-color: cyan;
}

.checkmark-gray {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  background-color: gray;
}

/* When the checkbox is checked, add a blue background */
.contain input:checked~.checkmark-gray {
  background-color: gray;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.contain input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.contain .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>
