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
    <section ref="section">
      仏国議会に於ける脚本検閲問題 ――ゴンクウルの『娼婦エリザ』―― 岸田國士
      　一八九〇年十二月二十二日、仏国上院に於ける予算質問中、議員アルガン君は、政府が民間の一小劇場に対して、年額五百法の補助を与へ、同劇場を推奨する意図を表示したことを攻撃した。
      　その劇場は即ち自由劇場であり、攻撃の理由は、同劇場の上演脚本が、屡々風紀を紊すものであるといふのである。
      　これに対して、時の文部省芸術局長ラルウメ君は、極力自由劇場の功績を賞揚し、一二脚本の選択を誤つたとしても、それは当局の信頼を傷けるものではない――殊に、補助金といふも、実は座席の予約にすぎず、監督官を派遣してその都度報告を得るために、必要な処置を取つたまでであると弁疏これ努めたが、アルガン君、いつかな聴き入れず、遂に最近問題になつた脚本の筋を述べて、盛に右党の老人連を憤慨させた。ラングル・ド・ボオマノワアルといふ侯爵議員は、「監督は巡査で沢山だ」と叫ぶなど、なかなかの騒ぎ。文部大臣も遂に見兼ねて、演壇に立ち、「アルガン君は、若い劇作家が、自由劇場によつてのみ、その才能を世に問ふことができるといふ事実に、お気付きないか」と食つてかかる。左翼の議席から拍手が起る。「もちろん、なかには良くないものもある」と云ふと、アルガン君すかさず、「皆良くない」とやり返す。大臣は「皆ではありません。しかも、今日まで上演した脚本の中には、なかなか注目に値するものが多いのであります……」そして遂に、「若し、諸君が、本案を否決されるやうなことがあれば、わが劇芸術のため由々しき大事であることを警告したいと思ひます」と見得を切り、遂に投票採決の結果、原案は無事通過。
    </section>

    <section ref="section2">
      （大正十一年十一月）
      　ロダンの「接吻」が公開を禁止されたとき、大分いろいろな議論が起こった。がその議論の多くは、検閲官を芸術の評価者ででもあるように考えている点で、根本に見当違いがあったと思う。
      　検閲官は芸術の解らない人であっても差支えない。彼の職務は或る作品がいかなる芸術的価値を持つかを定めることではなく、ただそれが公開される場合に公衆に対していかなる影響を及ぼすかを精確に判定し、その影響が社会の秩序を紊乱びんらんし善良な風俗を壊乱するようなものであった場合に、その公開を禁止することである。いかにすぐれた作品でも、もし実際に右のような影響を公衆に及ぼすとすれば、彼は当然その作品の公開を禁止してよい。彼に対してその作品の芸術的価値を説いて聞かせることはなんの意味をもなさない。たとい彼がその作品の芸術的価値を充分理解し得るようになったところで、公衆に対する作品の影響が依然として同一である限りは、彼はその禁止を解くことができない。
      　そこで中心の問題は、公衆に対する作品の影響が、果たして精確に判定せられているか否かの問題である。
    </section>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

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

export default Vue.extend({
  mounted() {
    const el = this.$refs.section as HTMLElement
    el.innerHTML = censorshipText(
      el.innerText,
      window.getComputedStyle(el).color
    )

    const el2 = this.$refs.section2 as HTMLElement
    el2.innerHTML = censorshipText(
      el2.innerText,
      window.getComputedStyle(el2).color
    )
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
  &:hover {
    .censor {
      background-position-y: 100%;
      background-position-x: 0;
    }
  }
}
</style>

<style scoped lang="scss">
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
