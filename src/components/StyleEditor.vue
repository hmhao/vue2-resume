<template>
  <div class="styleEditor" ref="container">
    <div class="code" v-html="validCode"></div>
    <pre v-html="highLightedCode"></pre>
  </div>
</template>

<script>
import Prism from 'prismjs'
export default {
  name: 'style-editor',
  data () {
    return {
      validCode: ''
    }
  },
  props: {
    code: '',
    isSkip: false
  },
  computed: {
    highLightedCode () {
      return Prism.highlight(this.code, Prism.languages.css)
    }
  },
  watch: {
    code (value) {
      let lastChar = value.charAt(value.length - 1)
      if (!value || /;|\n/.test(lastChar) || this.isSkip) {
        this.validCode = `<style>${value}</style>`
      }
    }
  },
  methods: {
    goBottom () {
      this.$refs.container.scrollTop = 100000
    }
  }
}
</script>

<style scoped>
  .code {
    display: none;
  }
</style>
