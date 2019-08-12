<template>
  <div class="main-page">
    <cale-comp ref="calendar" @changeDate="changeDate" @checkDay="checkDay"></cale-comp>
    <hr>
    <div class="result">
      <p> 当天日期：{{ Utility.formatDate(new Date(), 'yyyy年MM月dd日')}}</p>
      <p>切换月份后的当前日期：{{currentDateStr}}</p>
      <p>选中日期：{{currentDayStr}}</p>
      <p>跳转到指定日期：例如
        <select id="" @change="changeToDate($event)">
          <option value="">请选择</option>
          <option value="2019-01-02">2019-01-02</option>
          <option value="2018-05-13">2018-05-13</option>
          <option value="2014-07-24">2014-07-24</option>
          <option value="2016-08-18">2016-08-18</option>
        </select></p>

      <p>回到今天：<small>（如果当前显示的不是当前月份才会有这个按钮）</small></p>
    </div>
  </div>
</template>
<script>
import Utility from '@/utility'
import caleComp from '@/components/cale-comp'
export default {
  data() {
    return {
      Utility,
      currentDateStr: '',
      currentDayStr: ''
    }
  },

  methods: {
    // 跳转指定月份
    changeToDate(e) {
      const dom = e.target || e.srcElement
      const selected = dom.selectedOptions[0]
      this.currentDayStr = selected.value
      this.$refs.calendar.checkDays(selected.value)
    },
    // 切换上月或下月回调方法
    changeDate(strDate) {
      this.currentDateStr = strDate
    },
    /**
     * 选中某一天
     * dayItem：某一天整个数据对象
     */
    checkDay(dayItem) {
      this.currentDayStr = dayItem.currentDate
      console.log('可以根据选中的日期去进行数据查找及处理')
    }
  },
  components: {
    caleComp
  }
}
</script>
<style lang="less">
.result {
  padding: 10px;
  position: relative;
  float: left;
  p {
    line-height: 30px;
  }
}
</style>

