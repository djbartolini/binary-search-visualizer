<script setup lang="ts">
  const array = [1, 3, 7, 10, 13, 15, 17, 18, 20, 21, 22, 23, 25, 33, 35, 42, 45, 47, 50, 62]
</script>

<template>
  <div class="array-div">
    <div v-for="(num, i) in array">
      <div class="box">
        {{ num }}
      </div>
    </div>
  </div>
</template>

<style scoped>
  .array-div {
    margin-top: 25%;
    display: flex;
    flex-direction: row;
  }
  .box {
    border: 1px solid white;
    padding: 8px;
    width: 40px;
    height: 40px;
  }
</style>