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
    .input-container
      .input
        .row.row-one
          .button(v-for="i in [7, 8, 9]")
            p {{i}}
          .del
            p DEL
        .row.row-two
          .button(v-for="i in [4, 5, 6,'+']")
            p {{i}}
        .row.row-three
          .button(v-for="i in [1, 2, 3, '-']")
            p {{i}}
        .row.row-four
          .button(v-for="i in ['.', 0, '/', 'x']")
            p {{i}}
        .row.row-five
          .button-last-row.reset
            p RESET
          .button-last-row.equal
            p =
</template>

<script>
export default {
  data() {
    return {
      selectedTheme: "one",
    };
  },
  computed: {
    theme() {
      return {
        "--theme-primary": "$theme-" + this.selectedTheme + "-primary",
      };
    },
  },
};
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
.calc-container
  display: flex
  flex-direction: column
  width: 35vw
  height: 85vh
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
  flex: 0.3
  margin: 1.5rem 0
  border-radius: 0.5rem
.input-container
  flex: 1
  display: flex
  border-radius: 0.5rem
.row
  display: flex
  margin-bottom: 1.5rem
.input
  display: grid
  grid-template-columns: 1fr
  padding: 2rem 2rem 1rem
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
  .input-container
    background: $theme-one-secondary
  .button
    p
      color: $theme-one-text
    box-shadow: 0px 5px 2px $theme-one-key-shadow
    background: $theme-one-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-one-del-reset-shadow
    background: $theme-one-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-one-del-reset-shadow
    background: $theme-one-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 5px 2px $theme-one-action-shadow
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
  .input-container
    background: $theme-two-secondary
  .button
    p
      color: $theme-two-text
    box-shadow: 0px 5px 2px $theme-two-key-shadow
    background: $theme-two-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-two-del-reset-shadow
    background: $theme-two-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-two-del-reset-shadow
    background: $theme-two-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 5px 2px $theme-two-action-shadow
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
  .input-container
    background: $theme-three-secondary
  .button
    p
      color: $theme-three-text
    box-shadow: 0px 5px 2px $theme-three-key-shadow
    background: $theme-three-key
  .del
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-three-del-reset-shadow
    background: $theme-three-del-reset
  .reset
    p
      font-size: 1.2rem
      color: white
    box-shadow: 0px 5px 2px $theme-three-del-reset-shadow
    background: $theme-three-del-reset
  .equal
    p
      color: white
    box-shadow: 0px 5px 2px $theme-three-action-shadow
    background: $theme-three-action
</style>
