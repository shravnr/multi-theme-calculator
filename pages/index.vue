<template lang="pug">
.container(:class="{'theme-one': selectedTheme === 'one', 'theme-two': selectedTheme === 'two', 'theme-three': selectedTheme === 'three'}")
  .calc-container
    .header
      h1.top-text calc.
      .theme-toggle
        h6.top-text Theme
        .toggle-buttons
          .numbers
            p.top-text 1
            p.top-text 2
            p.top-text 3
          .toggle
            .one(@click="selectedTheme = 'one'")
              .active-toggle(v-if="selectedTheme === 'one'")
            .two(@click="selectedTheme = 'two'")
              .active-toggle(v-if="selectedTheme === 'two'")
            .three(@click="selectedTheme = 'three'")
              .active-toggle(v-if="selectedTheme === 'three'")
    .display
      h1(v-if="operatorHoldState") {{firstNumberEntered}}
      h1(v-else) {{numberFormatted}} 
    .input-container
      .input
        .row.row-one
          .button(v-for="i in [7, 8, 9]", @click="numberClicked(i)")
            p {{i}}
          .del(@click="deleteValue", tabindex="0", @keydown.esc="deleteValue")
            p DEL
        .row.row-two
          .button(v-for="i in [4, 5, 6,'+']", @click="numberClicked(i)", :class="{'operator-clicked': isInHoldState(i)}")
            p {{i}}
        .row.row-three
          .button(v-for="i in [1, 2, 3, '-']", @click="numberClicked(i)", :class="{'operator-clicked': isInHoldState(i)}")
            p {{i}}
        .row.row-four
          .button(v-for="i in ['.', 0, '/', 'x']", @click="numberClicked(i)", :class="{'operator-clicked': isInHoldState(i)}")
            p {{i}}
        .row.row-five
          .button-last-row.reset(@click="resetCalc")
            p RESET
          .button-last-row.equal(@click="numberClicked('=')")
            p =
</template>

<script>
import _ from 'lodash'

export default {
  data() {
    return {
      keyboardKeyPressed: null,
      selectedTheme: 'one',
      number: [],
      firstNumberEntered: 0,
      operatorEntered: null,
      secondNumberEntered: 0,
      operatorSelected: null,
    }
  },
  mounted() {
    window.addEventListener('keydown', (e) => {
      this.keyboardKeyPressed = e.key
    })
  },
  methods: {
    numberClicked(number) {
      if (_.isNaN(Number(number)) && number !== '.') {
        if (this.firstNumberEntered === 0) {
          this.firstNumberEntered = Number(this.numberFormatted)
        } else if (
          this.firstNumberEntered > 0 &&
          this.secondNumberEntered === 0
        ) {
          this.secondNumberEntered = Number(this.numberFormatted)
        }
        if (
          this.firstNumberEntered > 0 &&
          this.secondNumberEntered > 0 &&
          this.operatorEntered !== null
        ) {
          this.firstNumberEntered = this.result
          this.secondNumberEntered = 0
        }
        this.number = []
        this.operatorSelected = number === '=' ? null : number
      } else {
        this.number.push(number)
      }
    },
    isInHoldState(operator) {
      return this.operatorHoldState && this.operatorSelected === operator
    },
    deleteValue() {
      this.number = []
    },
    resetCalc() {
      this.number = []
      this.firstNumberEntered = 0
      this.secondNumberEntered = 0
      this.operatorEntered = null
      this.operatorSelected = null
    },
  },
  computed: {
    numberFormatted() {
      return _.join(this.number, '')
    },
    operatorHoldState() {
      return (
        this.firstNumberEntered > 0 &&
        this.secondNumberEntered === 0 &&
        this.number.length === 0
      )
    },
    result() {
      if (this.operatorEntered === '+') {
        return this.firstNumberEntered + this.secondNumberEntered
      } else if (this.operatorEntered === '-') {
        return this.firstNumberEntered - this.secondNumberEntered
      } else if (this.operatorEntered === '/') {
        return this.firstNumberEntered / this.secondNumberEntered
      } else if (this.operatorEntered === 'x') {
        return this.firstNumberEntered * this.secondNumberEntered
      }
    },
  },
  watch: {
    number() {
      if (!this.operatorHoldState) {
        this.operatorEntered = this.operatorSelected
      }
      if (this.number.length > 0 && this.operatorSelected === null) {
        this.firstNumberEntered = 0
      }
    },
    keyboardKeyPressed() {
      const operators = ['+', '-', '/', 'x', '.']
      if (
        operators.includes(this.keyboardKeyPressed) ||
        !_.isNaN(Number(this.keyboardKeyPressed))
      ) {
        this.numberClicked(this.keyboardKeyPressed)
      }
      if (
        this.keyboardKeyPressed === 'Enter' ||
        this.keyboardKeyPressed === '='
      ) {
        this.numberClicked('=')
      }
      if (this.keyboardKeyPressed === 'Backspace') {
        this.deleteValue()
      }
      if (this.keyboardKeyPressed === 'Escape') {
        this.resetCalc()
      }
    },
  },
}
</script>

<style lang="sass" scoped>
@import '~/assets/styles/main'
.container
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center
  height: 100vh
  h2
    margin: 0
  @media (max-width: 400px)
    height: 100vh
.calc-container
  display: flex
  flex-direction: column
  width: 35vw
  height: 85vh
  @media (max-width: 400px)
    width: 90%
.header
  flex: 0.1
  display: flex
  justify-content: space-between
  align-items: center
  h1
    margin: 0
    padding-top: 1rem
.theme-toggle
  display: flex
  h6
    margin: 0
    text-transform: uppercase
    margin-right: 1rem
    align-self: flex-end
.toggle-buttons
  display: flex
  flex-direction: column
.numbers
  display: flex
  padding: 0 0.5rem
  p
    margin: 0
    margin-bottom: 0.25rem
    font-size: 0.7rem
    margin-right: 0.8rem
    &:last-child
      margin-right: 0
.toggle
  display: flex
  height: 1.2rem
  border-radius: 10rem
  .one, .two, .three
    flex: 1
.active-toggle
  transition: 0.5s
  height: 0.75rem
  width: 0.75rem
  margin: 0.23rem 0 0 0.25rem
  border-radius: 100%
.display
  display: flex
  justify-content: flex-end
  align-items: center
  flex: 0.3
  margin: 1.5rem 0
  border-radius: 0.5rem
  padding-right: 1.5rem
  transition: 0.2s
  h1
    font-size: 2.5rem
.input-container
  flex: 1
  display: flex
  border-radius: 0.5rem
  transition: 0.2s
.row
  display: flex
  margin-bottom: 1.5rem
  &:last-child
    margin-bottom: 0
  @media (max-width: 400px)
    justify-content: space-between
.input
  width: 100%
  display: grid
  grid-template-columns: 1fr
  padding: 2rem 2rem 1rem
  @media (max-width: 400px)
    padding: 1.5rem
.operator-clicked
  &:last-child
    opacity: 0.6
.button, .del
  cursor: pointer
  p
    margin: 0
    margin-top: 0.5rem
    font-size: 1.8rem
  display: flex
  border-radius: 0.5rem
  justify-content: center
  align-items: center
  width: 6rem
  margin-right: 1rem
  transition: 0.2s
  &:hover
    opacity: 0.7
  @media (max-width: 400px)
    margin: 0
    width: 4.2rem
    p
      font-size: 1.5rem
.button-last-row
  cursor: pointer
  flex: 1
  p
    margin: 0
    margin-top: 0.5rem
    font-size: 1.5rem
  display: flex
  border-radius: 0.5rem
  justify-content: center
  align-items: center
  width: 6rem
  margin-right: 1rem
  @media (max-width: 400px)
    &:last-child
      margin-right: 0
.theme-one
  background-color: $theme-one-primary
  .top-text
    color: white
  .active-toggle
    background: $theme-one-action
  .toggle
    background: $theme-one-secondary
  .display
    background: $theme-one-screen
    h1
      color: white
  .input-container
    background: $theme-one-secondary
  .button
    p
      color: $theme-one-text
    box-shadow: 0px 4px 2px $theme-one-key-shadow
    background: $theme-one-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-one-del-reset-shadow
    background: $theme-one-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-one-del-reset-shadow
    background: $theme-one-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 4px 2px $theme-one-action-shadow
    background: $theme-one-action
.theme-two
  background-color: $theme-two-primary
  .top-text
    color: $theme-two-text
  .active-toggle
    background: $theme-two-action
  .toggle
    background: $theme-two-secondary
  .display
    background: $theme-two-screen
    h1
      color: $theme-two-text
  .input-container
    background: $theme-two-secondary
  .button
    p
      color: $theme-two-text
    box-shadow: 0px 4px 2px $theme-two-key-shadow
    background: $theme-two-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-two-del-reset-shadow
    background: $theme-two-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-two-del-reset-shadow
    background: $theme-two-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 4px 2px $theme-two-action-shadow
    background: $theme-two-action
.theme-three
  background-color: $theme-three-primary
  .top-text
    color: $theme-three-text
  .active-toggle
    background: $theme-three-action
  .toggle
    background: $theme-three-secondary
  .display
    background: $theme-three-screen
    h1
      color: $theme-three-text
  .input-container
    background: $theme-three-secondary
  .button
    p
      color: $theme-three-text
    box-shadow: 0px 4px 2px $theme-three-key-shadow
    background: $theme-three-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-three-del-reset-shadow
    background: $theme-three-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 4px 2px $theme-three-del-reset-shadow
    background: $theme-three-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 4px 2px $theme-three-action-shadow
    background: $theme-three-action
</style>
