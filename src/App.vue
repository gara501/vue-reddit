<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
:root {
  --color-gray: #2c3e50;
  --color-dark-gray: #2d2d2d;
  --color-darkest-gray: #333;
  --color-white: #fff;
}

html {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--color-gray);
}
.container {
  margin: 0 auto;
  max-width: 1200px;
}
.wrapper {
  display: flex;
  flex-flow: row;
}
.left-block {
  width: 30%;
}
.right-block {
  width: 70%;
}
</style>
