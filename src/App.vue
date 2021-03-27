<template>
  <div id="app">
    <div class="first-box">
      <input type="text" v-model="numberInput" />
    </div>

    <div class="second-box">
      <select v-model="calSelect">
        <option v-for="(item, index) in calList" :key="index"  :value="item">{{ item.name }}</option>
      </select>
    </div>

    <div class="third-box">
      {{ this.result }}
    </div>
  </div>
</template>

<script>

import debounce from 'debounce'

const checkPrime = (locNumber) => {
  if(isNaN(+locNumber)) return false;

  if (locNumber <= 1) return false;

  for (let i = 2; i < locNumber; i++) {
      if (locNumber % i == 0) return false;
  }

  return true;
}

const checkFibonacci = (locNumber) => {
  if(isNaN(+locNumber)) return false;

  const pfs = (x) => {
    let s = parseInt(Math.sqrt(x));
    return (s * s == x);
  }

  return pfs(5 * locNumber * locNumber + 4) || pfs(5 * locNumber * locNumber - 4);
}

export default {
  name: 'App',
  data(){
    return {
      delayInput: null,
      numberInput: 7,
      calSelect: 0,
      calList: [
        {
          name: 'isPrime',
          calFn: checkPrime
        },
        {
          name: 'IsFibonacci',
          calFn: checkFibonacci
        }
      ]
    }
  },
  watch:{
    numberInput: debounce( function(val) {
      if(+val < 0) {
        this.numberInput = Math.round(val * -1)
        return
      }else if(+val % 1){
        this.numberInput = Math.round(val)
      }
    }, 500 )
  },
  computed:{
    result: function () {
      return this.calSelect.calFn(this.numberInput)
    }
  },
  created(){
    this.calSelect = this.calList[0]
  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: row;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 600px;
}

.first-box {
  min-width: 200px;
  border-style: solid;
  border-width: 1px;
}

.second-box {
  flex: auto;
  min-width: 100px;
  border-style: solid;
  border-width: 1px;
}

.third-box {
  min-width: 300px;
  border-style: solid;
  border-width: 1px;
}
</style>
