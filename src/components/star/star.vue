<template>
    <div class="star" :class="starType">
        <span v-for="(itemClass,index) in itemClasses" :class="itemClass" :key="index" class="star-item"></span>
    </div>
</template>

<script>
// 星的长度
const LENGTH = 5

export default {
  props: {
    size: Number,
    score: Number
  },
  // methods是个方法，methods中的函数，必须要有一定的触发条件,比如你点击事件要执行一个方法
  methods: {

  },
  // computed用来监控自己定义的变量,是在dom加载后马上执行的,实时响应的，比如你要根据data里一个值随时变化做出一些处理，就用computed。
  computed: {
    starType () {
      return 'star-' + this.size
    },
    // 分数换算成星星状态
    itemClasses () {
      let result = []
      let score = Math.floor(this.score * 2) / 2
      let hasDecimal = score % 1 !== 0
      let integer = Math.floor(score)
      for (let i = 0; i < integer; i++) {
        result.push('on')
      }
      if (hasDecimal) {
        result.push('half')
      }
      while (result.length < LENGTH) {
        result.push('off')
      }
      return result
    }
  }
}
</script>

<style scoped>
    .star-item{
      display: inline-block;
      background-repeat: no-repeat;
    }
    .on{
      background-image: url('star48_on@3x.png');
    }
    .half{
      background-image: url('star48_half@3x.png');
    }
    .off{
      background-image: url('star48_off@3x.png');
    }
    /* 三种尺寸 */
    .star-48 .star-item{
      width: 20px;
      height: 20px;
      margin-right: 22px;
      background-size: 20px 20px;
    }
    .star-36{
      width: 15px;
      height: 15px;
      margin-right: 6px;
      background-size: 15px 15px;
    }
    .star-24{
      width: 10px;
      height: 10px;
      margin-right: 3px;
      background-size: 10px 10px;
    }
    .star-item:last-child{
      margin: 0 !important;
    }
</style>
