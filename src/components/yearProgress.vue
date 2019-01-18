<template>
  <div class="container">  
    <progress class="progress" :percent="percent" activeColor="#ea5a49"></progress>
    <p>{{year}}年已经过去<span class="span">{{days}}</span>天，<span class="span">{{percent}}</span>%</p>
  </div>
</template>

<script>
export default {
  name: 'yearProgress',
  methods: {
    // 判断是否为闰年
    isLeapYear () {
      if ((this.year % 4 === 0 && this.year % 100 !== 0) || this.year % 400 === 0) {
        return 366
      } else {
        return 365
      }
    }
  },
  computed: {
    // 获取当前年份
    year () {
      return new Date().getFullYear()
    },
    days () {
      let start = new Date()
      // dateObject.setMonth(month,day),mouth取值0~11,day取值1~31
      start.setMonth(0)
      start.setDate(1)
      let offset = new Date().getTime() - start.getTime()
      return parseInt(offset / 1000 / 60 / 60 / 24)
    },
    percent () {
      return (this.days * 100 / this.isLeapYear()).toFixed(1)
    }
  }
}
</script>

<style lang="stylus" scoped>
.container
  position relative
  .progress
    position absolute
    width 400rpx
    top 160rpx
  .span
    color #ea5a49

</style>
