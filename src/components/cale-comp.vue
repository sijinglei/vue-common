<template>
  <div class="calendar">
    <h1>我是日历组件</h1>
    <div class="newDate">
      <div class="nowDate">
        <div class="header">
          <div class="hd-left">
            <span @click="changeMonth(0)" title="上月">←</span>
            <span @click="initCalendar(true)" v-if="isCurrentDate" title="回到今天">
              今天
            </span>
          </div>
          <div class="hd-center"><em>{{currentYear}}</em>年&nbsp;<em>{{currentMonth}}</em>月</div>
          <div class="hd-right" @click="changeMonth(1)" title="下月"><span>→</span></div>
        </div>
        <div class="weeks">
          <span>Sun</span>
          <span>Mon</span>
          <span>Tue</span>
          <span>Wed</span>
          <span>Thu</span>
          <span>Fri</span>
          <span>Sat</span>
        </div>
        <ul class="month">
          <li v-for="(item,i) in calendarData" :key="i" @click="checkDate(item)" @mouseenter="mouseEnterEvent($event,item)" @mouseleave="mouseLeaveEvent($event,item)">
            <span class="needNum" :class="{'other-day':!item.isCurrentMonth,'today':item.isCurrentDay&&item.isCurrentMonth,'click':item.isClick}">{{item.value}}
              <i class="bluePoint" v-if="item.isHasData&&item.isCurrentMonth"></i>
            </span>
            <div v-show="item.isShowLayer" class="layer" @click.stop="openCale(item)">
              <i class="iconfont i-add-cale"></i><em>添加日程</em>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import Utility from '@/utility'
export default {
  data() {
    return {
      Utility,
      currentDate: Utility.formatDate(new Date(), 'yyyyMMdd'), // 当前时间
      currentYear: '', // 当前年
      currentMonth: '', // 当前月
      currentShowDate: '',
      calendarData: [] // 日期对象
    }
  },
  computed: {
    isCurrentDate() {
      return this.currentDate.substring(0, 6) !== this.currentShowDate.substring(0, 6)
    }
  },
  mounted() {
    this.initCalendar()
  },
  methods: {
    // 初始化日历
    initCalendar(isClickToday = false) {
      const nowDate = new Date()
      this.viewDate(nowDate, isClickToday)
    },
    // 初始化日期控件
    viewDate(nowDate, isClickToday = false) {
      this.currentShowDate = Utility.formatDate(nowDate, 'yyyyMMdd')
      const year = nowDate.getFullYear()
      const month = nowDate.getMonth() + 1
      console.log('当前月份', month)
      const currentDate = nowDate.getDate()
      this.currentYear = year
      this.currentMonth = month
      let dayNum = 0 // 当前月有多少天
      if (month === 1 || month === 3 || month === 5 || month === 7 || month === 8 || month === 10 || month === 12) {
        dayNum = 31
      } else if (month === 4 || month === 6 || month === 9 || month === 11) {
        dayNum = 30
      } else if (month === 2 && this.isLeapYear(year)) {
        dayNum = 29
      } else {
        dayNum = 28
      }
      this.calendarData = []
      var _firstDay = new Date(year, month - 1, 1) // 当前月第一天
      for (var i = 0; i < 42; i++) {
        let day = i + 1 - _firstDay.getDay() // 根据周几计算
        let _thisDate = new Date(year, month - 1, day)
        let _thisMonth = _thisDate.getMonth() + 1
        let _thisDay = _thisDate.getDate()
        let _currentDate = Utility.formatDate(_thisDate, 'yyyy-MM-dd')
        let curentDay = {
          isCurrentMonth: _thisMonth === month, // 是否是当月时间,只有当月才可以点击和移动变色
          isCurrentDay: _thisDay === currentDate, // 是否是当月的当天
          isClick: false,
          value: _thisDay,
          month: _thisMonth,
          longStrDate: _currentDate.replace(/-/g, ''),
          currentDate: _currentDate,
          isHasData: false // 是否存在数据
        }
        this.calendarData.push(curentDay)
      }
      if (isClickToday) {
        let item = this.calendarData.find(d => d.isCurrentDay)
        this.$emit('changeDate', item.currentDate)
      }
    },
    // 判断是否为闰年
    isLeapYear(year) {
      if (year % 4 === 0 && year % 100 !== 0) {
        return true
      } else {
        if (year % 400 === 0) {
          return true
        } else {
          return false
        }
      }
    },
    // 切换日期
    changeMonth(type) {
      let nowDate = new Date()
      let month = this.currentMonth
      if (type === 0) {
        month = this.currentMonth - 1
      } else {
        month = this.currentMonth + 1
      }
      nowDate.setFullYear(this.currentYear)
      nowDate.setMonth(month - 1)
      nowDate.setDate(nowDate.getDate())
      console.log('切换日期', nowDate)
      this.viewDate(nowDate)
      this.$emit('changeDate', Utility.formatDate(nowDate, 'yyyy-MM-dd'))
    },
    // 选中哪一天
    checkDate(item) {
      if (!item.isCurrentMonth) return
      this.calendarData.forEach(d => {
        //  && !item.isCurrentDay 如果当天就不选中
        d.isClick = d.value === item.value && d.month === item.month
      })
      this.$emit('checkDay', item)
    },
    checkDays(date) {
      this.viewDate(new Date(date))
    },
    // 赋值
    // initCaleData(caleData) {
    //   this.calendarData.forEach(p => {
    //     p.isHasData = false
    //     caleData.forEach(d => {
    //       const begin = +Utility.formatDate(d.BeginTime, 'yyyyMMdd')
    //       const end = +Utility.formatDate(d.EndTime, 'yyyyMMdd')
    //       let isHas = Number(p.longStrDate) >= begin && Number(p.longStrDate) <= end && p.isCurrentMonth
    //       if (isHas) {
    //         p.isHasData = isHas
    //       }
    //     })
    //   })
    // },
    mouseEnterEvent() {

    },
    mouseLeaveEvent() {

    }
  }
}
</script>


