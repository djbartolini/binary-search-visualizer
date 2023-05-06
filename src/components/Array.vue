<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue';
import type { StyleValue, CSSProperties, VNodeRef, VNodeProps } from '@vue/runtime-dom';

const array = ref<number[]>([1, 3, 7, 10, 13, 15, 17, 18, 20, 21, 22, 23, 25, 33, 35, 42, 45, 47, 50, 62]);
const key = ref<number>(33);
const middle = ref(0);
const keyPosition = ref('-48px');
const buttonText = ref<string>('Start');
const isSearchDone = ref<boolean>(false);

const inputArray = ref<string>('');
const inputKey = ref<string | undefined>('');


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
    width: `${array.value.length * 48}px`,
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

const handleChange = () => {
  inputArray.value = inputArray.value
    .replace(/[^0-9,\s]/g, '');
}

const handleKeyChange = () => {
  inputKey.value = inputKey.value
    ?.replace(/[^0-9]/g, '')
    .match(/\d+/g)
    ?.join('');
}

const insertionSort = (arr: number[], n: number) => { 
    let i, key, j; 
    for (i = 1; i < n; i++)
    { 
        key = arr[i]; 
        j = i - 1; 
        while (j >= 0 && arr[j] > key)
        { 
            arr[j + 1] = arr[j]; 
            j = j - 1; 
        } 
        arr[j + 1] = key; 
    } 
} 

const handleSubmit = () => {
  handleReset();
  array.value = inputArray.value.split(/[,\s]+/).map(Number);
  insertionSort(array.value, array.value.length);

  key.value = Number(inputKey.value);
}

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

      if (middle.value === end) {
        getMiddlePosition(middle.value);
        buttonText.value = 'Reset';
        isSearchDone.value = true;
        return;
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

console.log("input array", inputArray.value);
</script>

<template>
  <div>
    <div class="input-div">
      <p>Input values will be comma or space delimited</p>
      <form @submit.prevent="handleSubmit" class="form">
        <label>
          Input an array of numbers:
          <input v-model="inputArray" @input="handleChange" class="input-array" type="text"/>
        </label>
        <label>
          Input your search term:
          <input v-model="inputKey" @input="handleKeyChange" class="input-term" />
        </label>
        <button type="submit" class="submit-btn">Submit</button>
      </form>
    </div>
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
        {{ key }}
      </div>
    </div>
  </div>
  <div class="start-button-div">
    <button v-if="!isSearchDone" @click="handleSearch(array, key)" class="start-btn">Start</button>
    <button v-if="isSearchDone" @click="handleReset" class="start-btn">Reset</button>
  </div>
</template>

<style scoped>

.input-div {
  margin: 120px 0;
}

.input-div p {
  text-align: center;
}

.input-array {
  font-size: 16px;
  width: 600px;
  margin: 12px 24px;
  padding: 4px;
  background-color: #242424;
  color: white;
  border: 1px solid #696969;
  border-radius: 2px;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: start;
}

.input-term {
  font-size: 16px;
  width: 48px;
  margin: 0 0 0 45px;
  padding: 4px;
  background-color: #242424;
  color: white;
  border: 1px solid #696969;
  border-radius: 2px;
}

.submit-btn {
  width: 96px;
  margin: 0 auto;
}

.start-btn {
  width: 96px;
  margin: 0 auto;
}

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