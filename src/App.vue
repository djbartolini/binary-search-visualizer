<script setup lang="ts">
  import Array from './components/Array.vue'
</script>

<template>
  <header>
    <h1>Binary Search Visualizer</h1>
  </header>

  <main>
    <h2>Array:</h2>
    <div class="components">
      <Array />
    </div>
  </main>
</template>

<style scoped>
  header {
    display: flex;
    margin: 16px;
  }
  
  main {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h2 {
    margin: 12% auto 48px auto;
  }

  .components {
    width: fit-content;
  }

</style>
