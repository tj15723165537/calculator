<template>
  <div class="calculator">
    <div class="top">
      <input v-model="result"/>
    </div>
    <div class="main">
      <div class="key" v-for="item in keys" :key="item" @click="enter(item)">
        {{ item }}
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import {ref} from 'vue'

const result = ref<string>('0')
const keys = ref<string[]>(['←', 'C', 'x²', '÷', '7', '8',
  '9', '×', '4', '5', '6', '－', '1', '2', '3', '＋', '±', '0', '.', '＝'])
let currentMethod = undefined
const enter = (keyName) => {
  switch (keyName) {
    case '←':
      del()
      break;
    case '＋':
      add(keyName)
      break;
    case '＝':
      count()
      break;
    default:
      result.value = result.value + keyName
      if (result.value.startsWith('0')) {
        result.value = result.value.slice(1)
      }
  }
}
const del = () => {
  result.value = result.value.slice(0, -1)
  if (result.value === '') {
    result.value = '0'
  }
}
const add = (keyName) => {
  currentMethod = 'add'
  result.value = result.value + keyName
}
const count = () => {
  let total = 0
  switch (currentMethod) {
    case 'add':
      result.value.split('＋').forEach(item => {
        total += Number(item)
      })
    result.value = total
  }
}
</script>
<style lang="less">
@import './assets/css/styles.less';

.calculator {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 5px);
  padding-bottom: 5px;

  .top {
    input {
      outline: none;
      border: none;
      width: 100%;
      height: 30px;
      text-align: right;
      padding: 0 10px;
      box-sizing: border-box;
      font-size: 30px;
      font-weight: 700;
    }

    flex: 1;
    display: flex;
    align-items: center;
  }

  .main {
    height: 215px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-template-rows: 1fr;

    .key {
      padding: 8px 10px;
      border: 1px solid #c5dae2;
      margin: 2px;
      border-radius: 5px;
      text-align: center;
      font-size: 16px;
      font-weight: 500;
      background: #f8f9f9;
      cursor: default;
    }

    .key:active {
      box-shadow: #D6D6E7 0 3px 7px inset;
      transform: translateY(2px);
    }
  }
}
</style>
