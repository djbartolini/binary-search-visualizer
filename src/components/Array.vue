<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue';
import type { StyleValue, CSSProperties } from '@vue/runtime-dom';

const array = ref<number[]>([1, 3, 7, 10, 13, 15, 17, 18, 20, 21, 22, 23, 25, 33, 35, 42, 45, 47, 50, 62]);
const key = ref<number>(33);
const middle = ref(0);
const keyPosition = ref('-48px');
const buttonText = ref<string>('Start');
const isSearchDone = ref<boolean>(false);


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


const handleSearch = (array: number[], key: number): void => {
  let start: number = 0;
  let end: number = array.length - 1;
  let i: number = start;

  const searchStep = (): void => {
    if (start <= end) {
      middle.value = Math.floor((start + end) / 2);
      getMiddlePosition(middle.value);
      console.log('middle', middle.value);
  
      if (array[middle.value] === key) {
        buttonText.value = 'Reset';
        isSearchDone.value = true;
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
  setTimeout(searchStep, 100);
}

const handleReset = (): void => {
  keyPosition.value = '-48px';
  isSearchDone.value = false;
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
    <button v-if="!isSearchDone" @click="handleSearch(array, key)">Start</button>
    <button v-if="isSearchDone" @click="handleReset">Reset</button>
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

button {
  background-color: #242424;
  color: white;
  padding: 12px;
  border: 1px solid transparent;
  border-radius: 6px;
  transition: border 0.3s ease;
}

button:hover {
  border-color: rgb(30, 175, 105);
}
</style>