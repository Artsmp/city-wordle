<script setup lang="ts">
import Char from './components/Char.vue';
import pinyin from 'pinyin/lib/pinyin';
import confetti from 'canvas-confetti';
import { watch } from 'vue';
let text = $ref('');
let answers = ['厦门', '广州', '重庆', '成都', '武汉', '北京'];
let answer = $ref('厦门');
let win = $ref(false);
let history = $ref<Word[]>([]);
const handleClick = () => {
  history.push({
    text: text,
    py: pinyin(text, {
      segment: 'nodejieba',
    }),
  });
  text = '';
};

const randomAnswer = () => {
  answer = answers[Math.floor(Math.random() * 6 + 1)];
};

watch(history, (v) => {
  if (v.length) {
    if (history[history.length - 1].text === answer) {
      confetti({
        angle: 60,
        spread: 55,
        origin: { x: 0 },
      });
      confetti({
        angle: 120,
        spread: 55,
        origin: { x: 1 },
      });
      setTimeout(() => {
        win = true;
      }, 1000);
    }
  }
});

const computedStatus = (char: string, idx: number) => {
  const answers = answer.split('');
  if (answers.includes(char)) {
    if (answers[idx] === char) return 'right';
    return 'warning';
  } else {
    return 'normal';
  }
};

const handleTryAgain = () => {
  win = false;
  history.length = 0;
  randomAnswer();
};

let showAnswer = $ref(false);
const toggleShow = () => {
  showAnswer = !showAnswer;
};
</script>

<template>
  <main>
    <h1 text-red text-xl font-semibold my-3>City Wordle</h1>
    <template v-if="!win">
      <div
        @click="toggleShow"
        w-320px
        h-64
        bg-red-9
        border
        rounded
        flex
        justify-center
        items-center>
        答案是<span v-if="showAnswer" text-blue-4>{{ answer }}</span>
      </div>
      <div my-3>
        <input
          type="text"
          class="p-2"
          v-model="text"
          @keydown.enter="handleClick" />
        <button m="l-2" class="py-2 px-3 border border-red rounded-sm">
          输入
        </button>
      </div>
      <div
        class="flex gap-1 justify-center mb-2"
        v-for="prompt in history"
        :key="prompt.text">
        <Char
          v-for="(char, idx) in prompt.text"
          :text="{
            char,
            py: prompt.py[idx][0],
            status: computedStatus(char, idx),
          }" />
      </div>
    </template>
    <div flex text-green gap-2 flex-col justify-center items-center v-else>
      <div text-5 font-bold>You Win!</div>
      <button py-2 px-3 border border-green rounded-sm @click="handleTryAgain">
        再来一次
      </button>
    </div>
  </main>
</template>
