<template>
  <div id="app">
    <StyleEditor ref="styleEditor" 
      :code="currentStyle" 
      :isSkip="isSkip">
    </StyleEditor>
    <ResumeEditor ref="resumeEditor" 
      :markdown="currentMarkdown" 
      :enableHtml="enableHtml">
    </ResumeEditor>
    <AppFooter v-bind="{isStop, isSkip, scale}" 
      @pause="isStop = true" 
      @resume="isStop = false" 
      @skip="isSkip = true" 
      @replay="remake" 
      @accelerate="scale < 2 && scale++" 
      @decelerate="scale > -2 && scale--">
    </AppFooter>
  </div>
</template>

<script>
import StyleEditor from '@/components/StyleEditor'
import ResumeEditor from '@/components/ResumeEditor'
import AppFooter from '@/components/Footer'
import style from '@/assets/style.ecss'
import resume from '@/assets/resume.md'

export default {
  name: 'app',
  components: {
    StyleEditor,
    ResumeEditor,
    AppFooter
  },
  data () {
    return {
      timer: 0,
      scale: 0,
      isStop: false,
      isSkip: false,
      currentStyle: '',
      enableHtml: false,
      fullStyle: style.split('/*@@*/'),
      currentMarkdown: '',
      fullMarkdown: resume
    }
  },
  computed: {
    interval () {
      return Math.pow(2, -this.scale) * 20
    }
  },
  mounted () {
    this.makeResume()
  },
  methods: {
    makeResume: async function() {
      await this.showStyle(0)
      await this.showResume()
      await this.showStyle(1)
      await this.showHtml()
      await this.showStyle(2)
      this.makeComplete()
    },
    showStyle (n) {
      return new Promise((resolve, reject) => {
        let style = this.fullStyle[n]
        if (!style) {
          return reject()
        }
        // 计算前 n 个 style 的字符总数
        let length = this.fullStyle
          .filter(function (_, index) {
            return index <= n
          })
          .map(function (item) {
            return item.length
          })
          .reduce(function (p, c) {
            return p + c
          }, 0)
        let i = 0
        const progress = async function() {
          if (this.currentStyle.length < length && !this.isSkip) {
            let char = style.substring(i, i + 1) || ' '
            this.currentStyle += char
            // 遇到换行符则移动滚动条
            if (style.substring(i - 1, i) === '\n' && this.$refs.styleEditor) {
              this.$nextTick(() => {
                this.$refs.styleEditor.goBottom()
              })
            }
            i++
            while (this.isStop) {
              await this.promiseDelay(this.interval)
            }
            this.timer = setTimeout(progress, this.interval)
          } else { // 完成或跳过则将剩下的内容一次性赋值
            this.currentStyle += style.substring(i, style.length)
            resolve()
          }
        }.bind(this)
        this.timer = setTimeout(progress, this.interval)
      })
    },
    showResume () {
      return new Promise((resolve, reject) => {
        let markdown = this.fullMarkdown
        let length = markdown.length
        let i = 0
        const progress = async function() {
          if (this.currentMarkdown.length < length && !this.isSkip) {
            let char = markdown.substring(i, i + 1)
            this.currentMarkdown += char
            // 遇到换行符则移动滚动条
            if (markdown.substring(i - 1, i) === '\n' && this.$refs.resumeEditor) {
              this.$nextTick(() => {
                this.$refs.resumeEditor.goBottom()
              })
            }
            i++
            while (this.isStop) {
              await this.promiseDelay(this.interval)
            }
            this.timer = setTimeout(progress, this.interval)
          } else { // 完成或跳过则将剩下的内容一次性赋值
            this.currentMarkdown += markdown.substring(i, markdown.length)
            resolve()
          }
        }.bind(this)
        this.timer = setTimeout(progress, this.interval)
      })
    },
    showHtml () {
      return new Promise((resolve, reject) => {
        this.enableHtml = true
        resolve()
      })
    },
    makeComplete () {
      this.isSkip = true
    },
    remake () {
      this.isStop = false
      this.isSkip = false
      this.enableHtml = false
      this.currentStyle = ''
      this.currentMarkdown = ''
      this.makeResume()
    },
    promiseDelay (duration) {
      return new Promise(function(resolve) {
        setTimeout(function() {
          resolve()
        }, duration)
      })
    }
  }
}
</script>


<style>
  *{margin: 0; padding: 0; box-sizing: border-box;}
  *::before{box-sizing: border-box;}
  *::after{box-sizing: border-box;}
  html{min-height: 100vh;}
</style>
