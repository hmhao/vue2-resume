<template>
  <div class="footer">
    <span style="float:left" v-if="isComplete">
      <a href="javascript:void(0)" @click="$emit('enter')">&lt;-- 观看简历生成动画 --&gt;</a>
    </span>
    <span style="float:left" v-else>
      <span v-if="isSkip">
        <a href="javascript:void(0)" @click="$emit('exit')">&lt;-- 退出 --&gt;</a>
        <a href="javascript:void(0)" @click="$emit('replay')">重播 --&gt;</a>
      </span>
      <span v-else>
        <a href="javascript:void(0)" @click="$emit(isStop ? 'resume' : 'pause')">{{status}}</a>
        <a href="javascript:void(0)" @click="$emit('skip')">跳过动画 --&gt;</a>
        <a href="javascript:void(0)" @click="$emit('accelerate')">加速 +</a>
        <span>{{speed}}</span>
        <a href="javascript:void(0)" @click="$emit('decelerate')">- 减速</a>
      </span>
    </span>
    <span style="float:right">
      <a href="javascript:void(0)" v-if="isComplete" @click="$emit('download')">下载简历</a>
      <a href="https://hmhao.github.io/">博客</a>
      <a href="https://github.com/hmhao">GitHub</a>
      <a href="https://github.com/hmhao/vue2-resume">源码</a>
    </span>
  </div>
</template>

<script>
export default {
  name: 'footer',
  props: {
    isStop: Boolean,
    isSkip: Boolean,
    isComplete: Boolean,
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
