<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: none;
  font-family: Roboto, sans-serif;
  font-size: 10px;
}

ul {
  list-style-type: none;
}

body {
  background-color: rgb(10, 10, 30);
  height: 100vh;
}
</style>
