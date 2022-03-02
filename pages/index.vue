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

    .input
</template>

<script>
export default {
  data() {
    return {
      selectedTheme: "one",
    };
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
  height: 75vh
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
  padding: 0 0.25rem
  p
    margin: 0
    margin-bottom: 0.25rem
    font-size: 0.7rem
    margin-right: 0.8rem
    &:last-child
      margin-right: 0
.toggle
  display: flex
  height: 1.15rem
  border-radius: 10rem
  .one, .two, .three
    flex: 1
.active-toggle
  transition: 0.5s
  height: 0.8rem
  width: 0.8rem
  margin: 0.16rem 0 0 0.15rem
  border-radius: 100%
.display
  flex: 0.3
  margin: 1.5rem 0
  border-radius: 0.5rem
.input
  flex: 1
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
</style>
