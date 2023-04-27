<script setup lang="ts">
import type { StyleValue } from 'vue';

  const array = [1, 3, 7, 10, 13, 15, 17, 18, 20, 21, 22, 23, 25, 33, 35, 42, 45, 47, 50, 62];

  const binarySearch = (array: number[], key: number) => {
    let start = 0;
    let end = array.length - 1;

    while (start <= end) {
      let middle = Math.floor((start + end) / 2);

      if (array[middle] === key) {
        return middle;
      } else if (array[middle] < key) {
        start = middle++;
      } else {
        end = middle--;
      }
    }
  }

  const columns = (array: number[]): string => {
    let string = '';
    for (let i = 0; i < array.length; i++) {
      string = string.concat('auto ');
    }
    return string;
  }

  const gridStyle = (): StyleValue => {
    return {
      display: 'grid',
      gridTemplateColumns: `${columns(array)}` 
    }
  }

  console.log(columns(array));
  console.log(gridStyle());
  console.log(binarySearch(array, 33));

</script>

<template>
  <div class="array-div" :style="gridStyle()">
    <div v-for="(num, i) in array">
      <div class="indices">
        {{ i }}
      </div>
      <div class="box">
        {{ num }}
      </div>
    </div>
  </div>
  <div>
    <div class="term">
      {{ 33 }}
    </div>
  </div>
</template>

<style scoped>


  .indices {
    padding: 0 0 10px 18px;
  }

  .box {
    border: 1px solid white;
    width: 48px;
    height: 48px;
    text-align: center;
    padding-top: 12px;
  }

  .term {
    border: 1px solid white;
    width: 48px;
    height: 48px;
    text-align: center;
    padding-top: 12px;
    margin-top: 18px;
  }
</style>