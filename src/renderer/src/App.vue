<template>
  <div class="calculator">
    <div class="top">
      <input v-model="result" readonly/>
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
    case 'C':
      reset()
      break;
    case 'x²':
      square()
      break;
    case '±':
      negative()
      break;
    case '＋':
      add(keyName)
      break;
    case '－':
      reduce(keyName)
      break;
    case '×':
      multiply(keyName)
      break;
    case '÷':
      divide(keyName)
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
//删除
const del = () => {
  result.value = result.value.slice(0, -1)
  if (result.value === '') {
    result.value = '0'
  }
}
//重置
const reset = () => {
  result.value = '0'
  currentMethod = undefined
}
// 加法
const add = (keyName) => {
  if (currentMethod) count()
  currentMethod = '＋'
  result.value = result.value + keyName
}
// 减法
const reduce = (keyName) => {
  if (currentMethod) count()
  currentMethod = '－'
  result.value = result.value + keyName
}
//乘法
const multiply = (keyName) => {
  if (currentMethod) count()
  currentMethod = '×'
  result.value = result.value + keyName
}
//除法
const divide = (keyName) => {
  if (currentMethod) count()
  currentMethod = '÷'
  result.value = result.value + keyName
}
//乘方
const square = () => {
  result.value = Number(result.value) * Number(result.value)
}
// 负数vds
const negative = () => {
  let data = result.value
  if (currentMethod) {
    let [before, after] = data.split(currentMethod)
    if (after.startsWith('-')) {
      after = after.slice(1)
    } else {
      after = after.padStart(after.length + 1, '-')
    }
    result.value = before + currentMethod + after
  } else {
    if (data.startsWith('-')) {
      result.value = data.slice(1)
    } else {
      result.value = data.padStart(data.length + 1, '-')
    }
  }
}
const count = () => {
  switch (currentMethod) {
    case '＋':
      const [a, b] = result.value.split('＋')
      result.value = String(Number(a) + Number(b))
      break;
    case '－':
      const [c, d] = result.value.split('－')
      result.value = String(Number(c) - Number(d))
      break;
    case '×':
      const [e, f] = result.value.split('×')
      result.value = String(Number(e) * Number(f))
      break;
    case '÷':
      const [g, h] = result.value.split('÷')
      result.value = String(Number(g) / Number(h))
      break;
  }
  currentMethod = undefined
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
