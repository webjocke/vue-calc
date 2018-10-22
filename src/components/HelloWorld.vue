<template>
  <div class="calculator">
    <div class="display">{{ value || '0' }}</div>
    <div v-on:click="clear" class="button">AC</div>
    <div v-on:click="invert" class="button">+/-</div>
    <div v-on:click="procent" class="button">%</div>
    <div v-on:click="divide" v-bind:class="{ active: is_operand('divide')}" class="button operator">/</div>
    <div v-on:click="add('7')" class="button">7</div>
    <div v-on:click="add('8')" class="button">8</div>
    <div v-on:click="add('9')" class="button">9</div>
    <div v-on:click="times" v-bind:class="{ active: is_operand('times')}" class="button operator">*</div>
    <div v-on:click="add('4')" class="button">4</div>
    <div v-on:click="add('5')" class="button">5</div>
    <div v-on:click="add('6')" class="button">6</div>
    <div v-on:click="minus" v-bind:class="{ active: is_operand('minus')}" class="button operator">-</div>
    <div v-on:click="add('1')" class="button">1</div>
    <div v-on:click="add('2')" class="button">2</div>
    <div v-on:click="add('3')" class="button">3</div>
    <div v-on:click="plus" v-bind:class="{ active: is_operand('plus')}" class="button operator">+</div>
    <div v-on:click="add('0')" class="button zero">0</div>
    <div v-on:click="dot" class="button">.</div>
    <div v-on:click="equal" class="button operator">=</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      value: '',
      value_in_memory: '',
      operation: null,
      in_operation: false,
      clear_next: false
    }
  },
  methods: {
    add(num) {
      if (this.clear_next === true) {
        this.value = ''
        this.clear_next = false
      }
      this.value += num
    },
    is_operand(input) {
      if (input === this.in_operation) {
        return true
      } else {
        return false
      }
    },
    clear() {
      this.value = ''
      this.value_in_memory = ''
      this.operation = null
      this.in_operation = false
    },
    invert() {
      if (parseFloat(this.value) > 0 || parseFloat(this.value) < 0) {
        if (this.value.charAt(0) === '-') {
          this.value = this.value.slice(1)
        } else {
          this.value = '-' + this.value
        }
      }
      
    },
    procent() {
      this.value = this.value / 100
    },
    divide() {
      this.equal()
      this.value_in_memory = this.value
      this.in_operation = 'divide'
      this.operation = (a, b) => {
        return (parseFloat(a) / parseFloat(b)).toString()
      }
      this.clear_next = true
    },
    times() {
      this.equal()
      this.value_in_memory = this.value
      this.in_operation = 'times'
      this.operation = (a, b) => {
        return (parseFloat(a) * parseFloat(b)).toString()
      }
      this.clear_next = true
    },
    minus() {
      this.equal()
      this.value_in_memory = this.value
      this.in_operation = 'minus'
      this.operation = (a, b) => {
        return (parseFloat(a) - parseFloat(b)).toString()
      }
      this.clear_next = true
    },
    plus() {
      this.equal()
      this.value_in_memory = this.value
      this.in_operation = 'plus'
      this.operation = (a, b) => {
        return (parseFloat(a) + parseFloat(b)).toString()
      }
      this.clear_next = true
    },
    dot() {
      if (this.clear_next === true) {
        this.value = '0'
        this.clear_next = false
      }
      if (this.value === '') {
        this.value = '0'
      }
      if (this.value.indexOf('.') === -1) {
        this.value += '.'
      }
    },
    equal() {
      if (this.in_operation !== false) {
        this.value = this.operation(this.value_in_memory, this.value)
        this.in_operation = false
        this.value_in_memory = ''
        this.clear_next = true
      }
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    width: 350px;
    margin: auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(6, 1fr);
  }
  .button {
    background-color: #eeeeee;
    padding: 20px;
    font-size: 30px;
    border: 1px solid rgb(129, 129, 129);
  }
  .button:hover {
    cursor: pointer;
    -webkit-box-shadow: inset 0px 0px 30px 0px rgba(0,0,0,0.11);
    -moz-box-shadow: inset 0px 0px 30px 0px rgba(0,0,0,0.11);
    box-shadow: inset 0px 0px 30px 0px rgba(0,0,0,0.11);
  }
  .display {
    grid-column: 1 / 5;
    background-color: #555;
    text-align: right;
    font-size: 40px;
    color: white;
    padding: 14px 20px;
  }
  .zero {
    grid-column: 1 / 3;
  }
  .operator {
    background-color: orange;
    color: white;
  }
  .active {
    background-color: rgb(155, 102, 3);
  }
</style>
