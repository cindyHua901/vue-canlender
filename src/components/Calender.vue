<template>
  <div class="calender">
      <div class="tool-bar">
          <span id="prev-year" class="iconfont arrow-icon" @click="goLastYear">&#xe65e;</span>
          <span id="prev-month" class="iconfont arrow-icon" @click="goLastMonth">&#xe600;</span>
          <span id="next-month" class="iconfont arrow-icon" @click="goNextMonth">&#xe60a;</span>
          <span id="next-year" class="iconfont arrow-icon"  @click="goNextYear">&#xea25;</span>
      </div>
      <div class="title-bar">
        <div class="title-info">日</div>
        <div class="title-info">一</div>
        <div class="title-info">二</div>
        <div class="title-info">三</div>
        <div class="title-info">四</div>
        <div class="title-info">五</div>
        <div class="title-info">六</div>
      </div>
      <div class="calender-wapper"
        @click="handleClileItem($event)"
      >
        <CalenderItem
        v-for="(item, index) in itemList" :key="index"
        :item="item"
        :currentDay="inputDate"
        class="day"
      >
        </CalenderItem>
      </div>
  </div>
</template>

<script>
import CalenderItem from './CalenderItem'

export default {
  name: 'Calender',
  props: {
    date: Date,
    inputDate: Date
  },
  components: {
    CalenderItem
  },
  methods: {
    handleClileItem (e) { // 当点击日期格子的时候弹出格子代表的日期
      if (e.target.innerText) {
        alert(this.inputDate.getFullYear() + '年' + (this.inputDate.getMonth() + 1) + '月' + e.target.innerText + '日')
      }
    },
    goLastYear () {
      this.$emit('goToLastYear')
    },
    goNextYear () {
      this.$emit('goToNextYear')
    },
    goLastMonth () {
      this.$emit('goToLastMonth')
    },
    goNextMonth () {
      this.$emit('goToNextMonth')
    }
  },
  computed: {
    firstDate () { // 获取当月的第一天
      return new Date(this.inputDate.getFullYear(), this.inputDate.getMonth(), 1)
    },
    lastDate () { // 获取当月的最后一天
      return new Date(this.inputDate.getFullYear(), this.inputDate.getMonth() + 1, 0)
    },
    totalDays () { // 获取当月的总天数
      return this.lastDate.getDate()
    },
    startPos () { // 获取当月日期在日历中开始的位置
      return this.firstDate.getDay() + 1
    },
    totalItems () { // 计算总共日历格子的总数
      let calTotalbox = this.startPos + this.totalDays
      if (calTotalbox % 7 < 2) {
        return (Math.floor(calTotalbox / 7)) * 7
      }
      return (Math.floor(calTotalbox / 7) + 1) * 7
    },
    itemList () {
      let arr = []
      for (let i = 1; i <= this.totalItems; i++) { // 仅在当月日期的位置放入日期，其余位置放入0占位
        if (i < this.startPos) {
          arr.push('')
        } else if (i < this.startPos + this.totalDays) {
          arr.push(new Date(this.inputDate.getFullYear(), this.inputDate.getMonth(), (i - this.startPos) + 1))
        } else {
          arr.push('')
        }
      }
      return arr
    }
  }
}
</script>

<style scoped>
.calender {
width: 370px;
height: 350px;
}
.calender .tool-bar{
width: 370px;
display: flex;
padding: 5px 0;
}
.calender .tool-bar .arrow-icon{
flex: 1;
background: #c7f0db;
font-size: 24px;
color:#464159;
text-align: center;
}
.calender .title-bar{
width: 370px;
height: 30px;

}

.calender .title-bar .title-info{
float: left;
width: 50px;
height: 30px;
line-height: 30px;
text-align: center;
}
</style>
