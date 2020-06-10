<template>
  <div>
    <h1>#水曜GUI</h1>
    <h2>検閲エフェクトやりたい</h2>

    <!-- <section class="control">
      <label>
        <input type="range" />
      </label>
    </section> -->

    <!-- eslint-disable no-irregular-whitespace -->
    <div class="textList">
      <section
        v-for="({ text, color }, i) in textList"
        ref="section"
        :key="i"
        :style="{ color }"
      >
        {{ text }}
      </section>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { textList } from '@/assets/text'

const minLength = 20

function getChunkedText(text: string): string[] {
  const index = Math.random() * 10 + minLength
  const text1 = text.substring(0, index)
  const text2 = text.substring(index)
  if (text2.length < minLength) return [text]
  return [text1, ...getChunkedText(text2)]
}

function censorshipText(text: string, color: string): string {
  const splitedTextList = getChunkedText(text)
  return splitedTextList.reduce((acc, line) => {
    const textLength = line.length
    const censorLength = Math.ceil(Math.random() * textLength)
    const start = Math.floor(Math.random() * (textLength - censorLength))

    const replaced = line.replace(
      new RegExp(`(.{${start}})(.{${censorLength}})`),
      (_match, p1, p2) => {
        return `${p1}<span class="censor" style="background-image: linear-gradient(180deg, ${color} 50%, transparent 0)">${p2}</span>`
      }
    )

    return acc + replaced
  }, '')
}
function getRandomColor(): stiring {
  return '#' + (((1 << 24) * Math.random()) | 0).toString(16)
}
export default Vue.extend({
  data() {
    return {
      textList: textList.map((text) => {
        return {
          text,
          color: getRandomColor(),
        }
      }),
    }
  },
  mounted() {
    const sectionList = this.$refs.section as HTMLElement[]

    sectionList.forEach((el) => {
      el.innerHTML = censorshipText(
        el.innerText,
        window.getComputedStyle(el).color
      )
    })
  },
})
</script>

<style lang="scss">
.censor {
  // display: inline-block;
  // background-image: linear-gradient(180deg, #000 50%, transparent 0);
  background-size: auto 200%;
  transition: all 0.4s cubic-bezier(0.55, 0, 0.1, 1);
  background-position-y: 0px;
  background-position-x: 0;
}
section {
  writing-mode: vertical-rl;
  text-align: start;
  padding: 10px;
  font-family: 'Noto Serif JP', serif;
  line-height: 1.7;
  font-size: 20px;
  &:hover {
    .censor {
      background-position-y: 100%;
      background-position-x: 0;
    }
  }
}
</style>

<style scoped lang="scss">
.textList {
  display: flex;
  flex-direction: row-reverse;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
