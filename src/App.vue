<template>
  <div id="app">
    <div class="calculator">
      <div class="result" style="grid-area: result">
        {{ result  }}
      </div>

      <button class="non" style="grid-area: ac" @click="clear">AC</button>
      <button class="non" style="grid-area: plus-minus" @click="calculateToggle">+/-</button>
      <button class="non" style="grid-area: delete" @click="del">DEL</button>
      <button class="operator" style="grid-area: add" @click="append('+')">+</button>
      <button class="operator" style="grid-area: subtract" @click="append('-')">-</button>
      <button class="operator" style="grid-area: multiply" @click="append('×')">×</button>
      <button class="operator" style="grid-area: divide" @click="append('/')">/</button>
      <button class="operator" style="grid-area: equal" @click="calculate">=</button>

      <button class="number" style="grid-area: number-1" @click="append(1)">1</button>
      <button class="number" style="grid-area: number-3" @click="append(3)">3</button>
      <button class="number" style="grid-area: number-4" @click="append(4)">4</button>
      <button class="number" style="grid-area: number-2" @click="append(2)">2</button>
      <button class="number" style="grid-area: number-5" @click="append(5)">5</button>
      <button class="number" style="grid-area: number-6" @click="append(6)">6</button>
      <button class="number" style="grid-area: number-7" @click="append(7)">7</button>
      <button class="number" style="grid-area: number-8" @click="append(8)">8</button>
      <button class="number" style="grid-area: number-9" @click="append(9)">9</button>
      <button class="number" style="grid-area: number-0" @click="append(0)">0</button>
      <button class="number" style="grid-area: dot" @click="append('.')">.</button>
    </div>
  </div>
</template>

<script>
export default({
  el: '#app',
  name: 'App',
  data() {
    return {
      result : '0',
      isDecimalAdded: false,
      isOperatorAdded: false,
      isStarted: false,
    }
  },
  methods: {
    // operator check
    isOperator(character) {
      return ['+', '-', '×', '/'].indexOf(character) > -1 //existance true 
    },
    // Click Operators or Numbers
    append(character) {

      if(this.result.length > 9){
        return
      }
      // Start
      if (this.result === '0' && !this.isOperator(character)) {
        if (character === '.') {
          this.result += '' + character
          this.isDecimalAdded = true
        } else {
          this.result = '' + character
        }
        
        this.isStarted = true
        return
      }
      
      // Number
      if (!this.isOperator(character)) {
        if (character === '.' && this.isDecimalAdded) {
          return
        }
        
        if (character === '.') {
          this.isDecimalAdded = true
          this.isOperatorAdded = true
        } else {
          this.isOperatorAdded = false
        }
        
        this.result += '' + character
      }
      
      // Added Operator
      if (this.isOperator(character) && !this.isOperatorAdded) {
        this.result += '' + character
        this.isDecimalAdded = false
        this.isOperatorAdded = true
      }
    },
    // Click '='
    calculate() {
      let result= this.result.replace(new RegExp('×', 'g'), '*').replace(new RegExp('/', 'g'), '/')
      
      this.result = parseFloat(eval(result).toFixed(9)).toString()
      this.isDecimalAdded = false
      this.isOperatorAdded = false
    },
    // Click '+/-'
    calculateToggle() {
      if (this.isOperatorAdded || !this.isStarted) {
        return
      }
      
      this.result = this.result + '* -1'
      this.calculate()
    },
    // Click '%'
    calculatePercentage() {
      if (this.isOperatorAdded || !this.isStarted) {
        return
      }
      
      this.result = this.result + '* 0.01'
      this.calculate()
    },

    //Click 'DEL'
    del() {
      this.result = this.result.substr(0,this.result.length - 1);
      if(this.result.length == 0){
        this.result = '0'
      }
      
    },

    // CLick 'AC'
    clear() {
      this.result = '0'
      this.isDecimalAdded = false
      this.isOperatorAdded = false
      this.isStarted = false
    }
  }
})
</script>

<style>
body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.calculator {
  --button-width: 80px;
  --button-height: 80px;
  
  display: grid;
  grid-template-areas: "result result result result"
    "ac plus-minus delete divide"
    "number-7 number-8 number-9 multiply"
    "number-4 number-5 number-6 subtract"
    "number-1 number-2 number-3 add"
    "number-0 number-0 dot equal";
  grid-template-columns: repeat(4, var(--button-width));
  grid-template-rows: repeat(6, var(--button-height));
}

.calculator button {
  background-color:  #eee;
  border: 0.1px solid #333;
}

.calculator button:active {
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset, 4px 4px 10px -8px rgba(0, 0, 0, .3) inset;
}

.calculator .non {
  background-color: #eee;
}

.calculator .operator {
  background-color: orange;
  color: white;
}

.calculator .number {
  background-color: gray;
  color: white;
}

.result {
  background-color: #000;
  text-align: right;
  line-height: var(--button-height);
  font-size: 48px;
  font-family: Helvetica;
  padding: 0 20px;
  color: white;
  /*overflow-x:auto;*/
}
</style>
