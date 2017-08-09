<template>
  <div class="footer">
    <span style="float:left" v-show="!isSkip">
      <a href="javascript:void(0)" id="pause-resume" @click="$emit(isStop ? 'resume' : 'pause')">{{status}}</a>
      <a href="javascript:void(0)" id="skip-animation" @click="$emit('skip')">跳过动画 --&gt;</a>
      <a href="javascript:void(0)" id="accelerate" @click="$emit('accelerate')">加速 +</a>
      <span>{{speed}}</span>
      <a href="javascript:void(0)" id="decelerate" @click="$emit('decelerate')">- 减速</a>
    </span>
    <span style="float:left" v-show="isSkip">
      <a href="javascript:void(0)" id="replay" @click="$emit('replay')">&lt;-- 重播</a>
    </span>
    <span style="float:right">
      <a href="https://hmhao.github.io/">Blog</a>
      <a href="https://github.com/hmhao">GitHub</a>
      <a href="https://github.com/hmhao">View Source</a>
    </span>
  </div>
</template>

<script>
export default {
  name: 'footer',
  props: {
    isStop: Boolean,
    isSkip: Boolean,
    scale: Number
  },
  computed: {
    status () {
      return this.isStop ? '播放 >>' : '暂停 ||'
    },
    speed () {
      return `${this.scale}X`
    }
  }
}
</script>

<style>
  .footer{
    position: absolute;
    bottom: 0;
    height: 20px;
    left: 0;
    right: 0;
    padding: 0 10px;
  }
</style>
