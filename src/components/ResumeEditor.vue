<template>
  <div class="resumeEditor" :class="{htmlMode:enableHtml, resumeComplete:isComplete}" ref="container">
    <div v-if="enableHtml" v-html="result"></div>
    <pre v-else>{{result}}</pre>
  </div>
</template>

<script>
import marked from 'marked'
const rendererMD = new marked.Renderer()
const regex = /^\[side-(left|right)-(start|end)\]$/g
rendererMD.paragraph = function (text) {
  let match = regex.exec(text)
  regex.lastIndex = 0
  if (match) {
    if (match[2] === 'start') {
      return `<div class="side-${match[1]}">`
    } else {
      return `</div>`
    }
  } else {
    return `<p>${text}</p>\n`
  }
}
marked.setOptions({
  renderer: rendererMD
})

export default {
  name: 'resume-editor',
  props: {
    markdown: '', 
    enableHtml: false,
    isComplete: true
  },
  computed: {
    result () {
      return this.enableHtml ? marked(this.markdown) : this.markdown
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
  .htmlMode {
    animation: flip 2s;
  }
  
  @keyframes flip {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  .resumeComplete{
    max-width: 960px;
    height: 95vh;
    right: 25%;
    transform: rotateY(0) translateZ(0);
  }
</style>
