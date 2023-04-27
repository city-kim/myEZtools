<script setup lang="ts">
import { ref } from 'vue'

import IconUp from '@/components/icon/iconUp.vue'
import IconDown from '@/components/icon/iconDown.vue'

// 리스트 아이템의 타입
interface ListItem {
  index: number
  rate: number
}

const count = ref(2) // 숫자 비교범위

const result = ref<null|'diff'|'list'>(null) // 결과 출력여부

const diff = ref<ListItem>({index: 0, rate: 0}) // 2개씩 비교
const list = ref<Array<ListItem>>([]) // 리스트만들기

function pick () {
  if (count.value > 2) {
    // 카운트가 2를 넘기면 리스트업
    countOfList()
  } else {
    // 카운트가 2를 넘기지 않으면 둘중에 하나 비교
    trueOrFalse()
  }
}

function trueOrFalse () {
  // 둘중에 하나 비교하기
  
  const rate = Math.random() * 100 // 랜덤 확률

  // 50이 넘을경우 2번, 아닐경우 1번
  diff.value = {
    index: rate > 50 ? 2: 1,
    rate: rate
  }
  result.value = 'diff'
}

function countOfList () {
  // 숫자에 따라서 리스트업하기
  list.value = Array(count.value).fill({index: 0, rate: 0})
  .map((_, index) => {
    return {
      // 1부터 시작
      index: index + 1,
      rate: Math.random() * 10000
    }
  })
  .sort((a, b) => {
    // 확률에 따라서 정렬
    return b.rate - a.rate
  })
  result.value = 'list'
}
</script>
<template>
  <div class="ct-picker">
    <fieldset
      v-show="!result"
    >
      <div>
        <button
          type="button"
          @click="count > 2 ? count -- : count = 2"
        >
          <IconDown></IconDown>
        </button>
        <input
          v-model="count"
          type="number"
        />
        <button
          type="button"
          @click="count++"
        >
          <IconUp></IconUp>
        </button>
      </div>
      <button
        type="button"
        @click="pick()"
      >Pick</button>
      <p>숫자가 2인경우 1 또는 2 yes or no를 출력함</p>
      <p>숫자가 n인경우 점수를 매겨서 순위를 출력함</p>
    </fieldset>
    <div
      v-show="result"
      class="result"
    >
      <div
        v-show="result == 'diff'"
        class="printer"
        :class="[diff.index > 1 ? 'false' : 'true']"
      >
        <h2>{{ diff.index }}번</h2>
        <h3>{{ diff.index > 1 ? '하지마라' : '해라' }}</h3>
        <p>{{ Math.floor(diff.rate) }}%</p>
        <button
          type="button"
          @click="result = null"
        >reset</button>
      </div>
      <div
        v-show="result == 'list'"
        class="list"
      >
        <ul>
          <li
            v-for="item in list"
            :key="item.index"
          >
            <strong>{{ item.index }}번</strong>
            <span>{{ Math.floor(item.rate) }} 점</span>
          </li>
        </ul>
        <button
          type="button"
          @click="result = null"
        >reset</button>
      </div>
    </div>
  </div>
</template>