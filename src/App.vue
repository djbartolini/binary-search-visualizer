<script setup lang="ts">
  import Array from './components/Array.vue'
</script>

<template>
  <header>
    <h1>Binary Search Visualizer</h1>
  </header>

  <main>
    <h2>Array:</h2>
    <Array />
    <div class="start-button-div">
      <button>Start</button>
    </div>
  </main>
</template>

<style scoped>
  header {
    display: flex;
  }
  main {
    min-height: 100vh;
  }
  .start-button-div {
    position: absolute;
    bottom: 20%;
  }
</style>
