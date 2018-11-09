<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
/*eslint-disable*/
export default {
  name: 'App'
}
document.addEventListener('DOMContentLoaded', function () {
  const html = document.querySelector('html')
  let fontSize = window.innerWidth / 10;
  fontSize = fontSize>36?36:fontSize 
  html.style.fontSize = fontSize + 'px'
})
</script>

<style>
</style>
