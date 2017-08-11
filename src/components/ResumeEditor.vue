<template>
  <div class="resumeEditor" :class="{htmlMode:enableHtml, resumeComplete:isComplete}" ref="container">
    <div v-if="enableHtml" v-html="result"></div>
    <pre v-else>{{result}}</pre>
  </div>
</template>

<script>
import marked from 'marked'
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
