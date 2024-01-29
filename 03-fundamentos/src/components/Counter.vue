<template>
  <h2>{{ title || 'Counter' }}</h2>
  <h2>customTitle: {{ title || 'Counter' }}</h2>
  <p>methods: {{ counter }} <sup>2</sup> = {{ getSquareValue() }}</p>
  <p>computed: {{ counter }} <sup>2</sup> = {{ squareCounter }}</p>

  <div>
    <button @click="increase">+1</button>
    <button @click="decrease">-1</button>
  </div>
</template>

<script>
export default {
  name: 'Counter',
  props: {
    title: String,
    start: {
      type: Number,
      default: 5,
      validator(value) {
        return value >= 0
      }
      // required: true
    }
  },
  data() {
    return {
        counter: this.start
    }
  },
  methods: {
    getSquareValue() {
      console.log('From getSquareValue()')
      return this.counter * this.counter
    },
    increase() {
      this.counter++
    },
    decrease() {
      this.counter--
    }
  },
  computed: {
    squareCounter() {
      console.log('From squareCounter')
      return this.counter * this.counter
    },
    customTitle() {
      return this.title ? this.title : 'Counter'
    }
  }
}
</script>

<style>

</style>