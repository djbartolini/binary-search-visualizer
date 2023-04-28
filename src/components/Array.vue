<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue';
import type { StyleValue, CSSProperties } from '@vue/runtime-dom';

const array = ref([1, 3, 7, 10, 13, 15, 17, 18, 20, 21, 22, 23, 25, 33, 35, 42, 45, 47, 50, 62]);
const middle = ref(0);
const keyPosition = ref('-48px');

const binarySearch = (array: number[], key: number) => {
  let start = 0;
  let end = array.length - 1;

  while (start <= end) {
    middle.value = Math.floor((start + end) / 2);

    if (array[middle.value] === key) {
      return middle;
    } else if (array[middle.value] < key) {
      start = middle.value++;
    } else {
      end = middle.value--;
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


const getMiddlePosition = (middle: number): void => {
  const middlePosition = middle * 48;
  keyPosition.value = middlePosition + 'px';
}


const gridStyle = (): StyleValue => {
  return {
    display: 'grid',
    gridTemplateColumns: `${columns(array.value)}`
  }
}

const keyStyle = computed((): CSSProperties => {
  return {
    border: '1px solid white',
    width: '48px',
    height: '48px',
    textAlign: 'center',
    paddingTop: '12px',
    marginTop: '18px',
    position: 'relative',
    left: `${keyPosition.value}`,
    transition: 'left 0.5s ease-in' 
  }
})

watchEffect(() => {
  keyStyle.value.left = `${keyPosition.value}`
})

let increment: number = 0;

const handleMove = (array: number[], key: number): void => {
  let start: number = 0;
  let end: number = array.length - 1;
  let i: number = start;

  const searchStep = (): void => {
    if (start <= end) {
      middle.value = Math.floor((start + end) / 2);
      getMiddlePosition(middle.value);
      console.log('middle', middle.value);
  
      if (array[middle.value] === key) {
        return;
      } else if (array[middle.value] < key) {
        start = middle.value++;
      } else {
        end = middle.value--;
      }

      i++;
      setTimeout(searchStep, 600);
    }
  }
  setTimeout(searchStep, 600);
}


</script>

<template>
  <div>
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
      <div :style="keyStyle">
        {{ 33 }}
      </div>
    </div>
  </div>
  <div class="start-button-div">
    <button @click="handleMove(array, 33)">Start</button>
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

.start-button-div {
  position: relative;
  left: 50%;
  top: 48px;
}
</style>