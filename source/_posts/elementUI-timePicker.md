---
title: elementUI 时间选择器组件二次封装
date: 2019-11-05 10:48:46
tags: [前端, Vue, ElementUI, 组件]
---

基于elmentUI时间选择器组件二次封装的组件
<!--more-->

# TimePicker组件

template：
```
<template>
  <el-date-picker
    v-model="timeValue"
    align="right"
    unlink-panels
    :range-separator="rangeSeparator" // 时间范围分隔符 String default:'-'
    :start-placeholder="startPlaceholder" // 开始时间的提示文字 String default:'开始时间'
    :end-placeholder="endPlaceholder" // 结束时间的提示文字 String default:'结束时间'
    :default-time="defaultTime" // 默认的时间值 Array default:['00:00:00', '23:59:59']
    :type="type" // 日期类型 daterange-日期范围 datetimerange-日期时间范围 String default:'daterange'
    :time-type="timeType" // 时间维度 0-日 1- 周 2-月 3-年 -1-时 String default:'0'
    :picker-type="pickerType" // 1-从今日开始 2-从昨日开始 3-从近七日开始 String default:'1'
    :need-options="needOptions" // 是否需要快捷选项 Boolean default:true
    :picker-options="pickerOptions" // 时间选择器的一些选项 Object default:{}
    @change="handlePick"
  />
</template>
```

import:
```
import $common from '@/utils/common.js'  // 文末附上common.js
```

props：
```
props: {
    // 时间范围分隔符
    rangeSeparator: {
      type: String,
      default: '-'
    },
    // 开始时间的文字说明
    startPlaceholder: {
      type: String,
      default: '开始时间'
    },
    // 结束时间的文字说明
    endPlaceholder: {
      type: String,
      default: '结束时间'
    },
    // 默认的时间点
    defaultTime: {
      type: Array,
      default: () => ['00:00:00', '23:59:59']
    },
    // 日期类型 daterange-日期范围 datetimerange-日期时间范围
    type: {
      type: String,
      default: 'daterange'
    },
    // 时间维度 0-日 1- 周 2-月 3-年 -1-时
    timeType: {
      type: String,
      default: '0'
    },
    // 1-从今日开始 2-从昨日开始 3-从近七日开始
    pickerType: {
      type: String,
      default: '1'
    },
    // 需要快捷选项
    needOptions: {
      type: Boolean,
      default: true
    },
    // 时间选择器的一些选项
    pickerOptions: {
      type: Object,
      default: () => { return {} }
    },
    value: {
      type: Array,
      default: () => []
    }
  }
```

data:
```
  data() {
    return {
      timeValue: this.value
    }
  }
```

methods:
```
  methods: {
    // 确定选择
    handlePick() {
      let result = ['', '']
      if (this.timeValue !== null) {
        const startTime = this.$timeUtils.getTime(this.timeValue[0])
        const endTime = this.$timeUtils.getTime(this.timeValue[1])
        result = [startTime, endTime]
      }
      this.$emit('change', result)
    },
    // 重置
    reset() {
      this.timeValue = ['', '']
      this.handlePick()
    }
  }
```


## 一、Picker Options

|参数|说明|类型|可选值|默认值|
|--|--|--|--|--|
|shortcuts|设置快捷选项，需要传入 { text, onClick } 对象用法参考 demo 或下表|Object[]|-|-|
|disabledDate|设置禁用状态，参数为当前日期，要求返回 Boolean|Function|-|-|
|cellClassName|设置日期的 className|Function(Date)|-|-|
|firstDayOfWeek|周起始日|Number|1 到 7|7|


### 1.设置快捷选项 shortcuts

- 这里增加了根据时间维度生成快捷选项

methods：
```
optionsGet() {
  const options = []
  const timeType = parseInt(this.timeType)
  const pickerType = parseInt(this.pickerType)

  if (!this.needOptions) return

  if (timeType === 0 || timeType === -1) {
    // 时间维度为 日 或 时
    if (pickerType <= 1) {
      options.push(
        {
          text: '今日',
          onClick(picker) {
            const start = new Date($common.getDateStr(0) + ' 00:00:00')
            const end = new Date($common.getDateStr(0) + ' 23:59:59')
            picker.$emit('pick', [start, end])
          }
        }
      )
    }
    if (pickerType <= 2) {
      options.push(
        {
          text: '昨日',
          onClick(picker) {
            const start = new Date($common.getDateStr(-1) + ' 00:00:00')
            const end = new Date($common.getDateStr(-1) + ' 23:59:59')
            picker.$emit('pick', [start, end])
          }
        }
      )
    }
    if (pickerType <= 3) {
      options.push(
        {
          text: '近7日',
          onClick(picker) {
            const start = new Date($common.getDateStr(-7) + ' 00:00:00')
            const end = new Date($common.getDateStr(-1) + ' 23:59:59')
            picker.$emit('pick', [start, end])
          }
        }
      )
    }
    options.push({
      text: '近30日',
      onClick(picker) {
        const start = new Date($common.getDateStr(-30) + ' 00:00:00')
        const end = new Date($common.getDateStr(-1) + ' 23:59:59')
        picker.$emit('pick', [start, end])
      }
    }, {
      text: '近90日',
      onClick(picker) {
        const start = new Date($common.getDateStr(-90) + ' 00:00:00')
        const end = new Date($common.getDateStr(-1) + ' 23:59:59')
        picker.$emit('pick', [start, end])
      }
    })
  } else if (timeType === 1) {
    // 时间维度为 周
    options.push(
      {
        text: '近4周',
        onClick(picker) {
          const start = new Date($common.getWeekDateStr(-28) + ' 00:00:00')
          const end = new Date($common.getWeekDateStr(-7) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
      {
        text: '近15周',
        onClick(picker) {
          const start = new Date($common.getWeekDateStr(-105) + ' 00:00:00')
          const end = new Date($common.getWeekDateStr(-7) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
      {
        text: '近30周',
        onClick(picker) {
          const start = new Date($common.getWeekDateStr(-210) + ' 00:00:00')
          const end = new Date($common.getWeekDateStr(-7) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
    )
  } else if (timeType === 2) {
    // 时间维度为 月
    options.push(
      {
        text: '近3个月',
        onClick(picker) {
          const start = new Date($common.getMonthDateStr(-90) + ' 00:00:00')
          const end = new Date($common.getMonthEndDateStr(-30) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
      {
        text: '近6个月',
        onClick(picker) {
          const start = new Date($common.getMonthDateStr(-180) + ' 00:00:00')
          const end = new Date($common.getMonthEndDateStr(-30) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
      {
        text: '近12个月',
        onClick(picker) {
          const start = new Date($common.getMonthDateStr(-360) + ' 00:00:00')
          const end = new Date($common.getMonthEndDateStr(-30) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      }
    )
  } else if (timeType === 3) {
    // 时间维度为 年
    options.push(
      {
        text: '今年',
        onClick(picker) {
          const start = new Date(new Date().getFullYear() + '-01' + '-01' + ' 00:00:00')
          const end = new Date($common.getDateStr(-1) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      },
      {
        text: '近3年',
        onClick(picker) {
          const start = new Date($common.getYearDateStr(-730) + ' 00:00:00')
          const end = new Date($common.getDateStr(-1) + ' 23:59:59')
          picker.$emit('pick', [start, end])
        }
      }
    )
  }

  this.pickerOptions.shortcuts = options
}
```

mounted：
```
mounted() {
  this.optionsGet()
},
```

### 2.设置可选时间范围 disabledDate

- 这里将其设置为prop，可在使用时设置范围

template:
```
<el-form-item label="选择时间">
  <timePicker
    type="datetimerange"
    :value="[new Date(form.startTime),new Date(form.endTime)]"
    time-type="0"
    :need-options="false"
    :picker-options="timePickerOptions"
    style="width:487px"
    @change="handleTimePicker"
  />
</el-form-item>
```

import:
```
import timePicker from '@/components/TimePicker/index.vue'
```

components:
```
components: {
  timePicker
},
```

data:
```
timePickerOptions: {
  disabledDate(time) {
    // return time.getTime() < Date.now() - 8.64e7// 当天（含）之后的时间可选
    // return time.getTime() > Date.now() - 8.64e7// 当天（不含）之前的时间可选
    // return time.getTime() > Date.now() // 当天（含）之前的时间可选
  }
}
```





## 附：timeUtils.js

```
export default {
  getTime: function getTime(t) { // 时间戳转时间文字
    if (t === undefined || t == null || t === '' || t === 0) {
      return ''
    }
    var time = new Date(t)
    var y = time.getFullYear()
    var M = this.get2(time.getMonth() + 1)
    var d = this.get2(time.getDate())

    var h = this.get2(time.getHours())
    var m = this.get2(time.getMinutes())
    var s = this.get2(time.getSeconds())
    return y + '-' + M + '-' + d + ' ' + h + ':' + m + ':' + s
  },
  getDate: function getDate(t) { // 时间戳转日期，用-隔开
    var time = new Date(t)
    var y = time.getFullYear()
    var M = this.get2(time.getMonth() + 1)
    var d = this.get2(time.getDate())
    return y + '-' + M + '-' + d
  },
  getDateSlant: function getDateSlant(t) { // 时间戳转日期，用/隔开
    var time = new Date(t)
    var y = time.getFullYear()
    var M = this.get2(time.getMonth() + 1)
    var d = this.get2(time.getDate())
    return y + '/' + M + '/' + d
  },
  getWeekNum: function getWeekNum(t) { // 获得当前周是本年的第几周，格式：2018年第01周
    var time = new Date(this.getSundayStamp(t))
    var firstDay = new Date(this.getSundayStamp(t))
    firstDay.setMonth(0)
    firstDay.setDate(1)
    var diff = time - firstDay
    var days = Math.ceil(diff / (24 * 60 * 60 * 1000))
    var weekNum = time.getFullYear() + '年第' + this.get2(Math.ceil(days / 7)) + '周'
    return weekNum
  },
  getMonthNum: function getMonthNum(t) { // 获得当前月，格式2018年01月
    var time = new Date(t)
    var monthNum = time.getFullYear() + '年' + this.get2(time.getMonth() + 1) + '月'
    return monthNum
  },
  getSundayStamp: function getSundayStamp(t) { // 获得当前日的周日时间戳
    var time = new Date(t)
    var day = time.getDay()
    var sunday = 0
    if (day === 0) {
      sunday = t
    } else {
      sunday = (7 - day) * (24 * 60 * 60 * 1000) + t
    }
    return sunday
  },
  get2: function get2(intValue) { // 时间的不足10补0
    if (intValue >= 0 && intValue <= 9) { return '0' + intValue }
    return intValue
  },
  timesCompare: function timesCompare(timeLittle, timeBig) { // 比较时间
    var result = 0
    timeLittle = new Date(timeLittle).getTime()
    timeBig = new Date(timeBig).getTime()
    if (timeLittle > timeBig) {
      result = -1
    }
    return result
  },
  hourFormat: function hourFormat(hour) { // 小时的格式化，格式：01:00
    var result = ''
    if (hour >= 0 && hour < 10) {
      result = '0' + hour + ':00'
    } else if (hour >= 10 && hour <= 24) {
      result = hour + ':00'
    }
    return result
  },
  timeToStamp: function timeToStamp(timeString) { // 日期转时间戳，日期格式：2018-01-01 00:00:00
    if (timeString === '') {
      return ''
    }
    var date = new Date(timeString.replace(/-/g, '/'))
    return date.getTime()
  },
  datetiemToString: function tiemToString(datetime) {
    var y = datetime.getFullYear()
    var M = this.get2(datetime.getMonth() + 1)
    var d = this.get2(datetime.getDate())

    var h = this.get2(datetime.getHours())
    var m = this.get2(datetime.getMinutes())
    var s = this.get2(datetime.getSeconds())
    return y + '-' + M + '-' + d + ' ' + h + ':' + m + ':' + s
  },
  datetiemToDateString: function datetiemToDateString(datetime) {
    var y = datetime.getFullYear()
    var M = this.get2(datetime.getMonth() + 1)
    var d = this.get2(datetime.getDate())
    return y + '-' + M + '-' + d
  },
  arrayIsNotNull: function arrayIsNotNull(array) {
    var result = 0
    if (array !== undefined && array !== null && array.length !== 0) {
      result = 1
    }
    if (array === undefined || array === null || array.length === 0) {
      result = -1
    }
    return result
  },
  objIsNotNull: function objIsNotNull(obj) {
    var result = 0
    if (obj !== undefined && obj !== null && JSON.stringify(obj) !== '{}') {
      result = 1
    }
    if (obj === undefined || obj === null || JSON.stringify(obj) === '{}') {
      result = -1
    }
    return result
  },
  strIsNotNull: function strIsNotNull(str) {
    var result = 0
    if (str !== undefined && str !== null && str !== '') {
      result = 1
    }
    if (str === undefined || str === null || str === '') {
      result = -1
    }
    return result
  }
}

```

## 附：common.js

```
import $timeUtils from './timeUtils.js'
export default {
  // 获取近x日的日期
  getDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)
    return $timeUtils.getDate(date)
  },
  // 获取近x周的开始日期
  getWeekDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)
    const nowDay = date.getDay()
    if (nowDay !== 1) {
      const minDay = -(nowDay - 1)
      date.setDate(date.getDate() + minDay)
    }
    return $timeUtils.getDate(date)
  },
  // 获取近x周的结束日期
  getWeekEndDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)
    const nowDay = date.getDay()
    if (nowDay !== 0) {
      const minDay = -(nowDay - 7)
      date.setDate(date.getDate() + minDay)
    }
    return $timeUtils.getDate(date)
  },
  // 获取近x月的开始日期
  getMonthDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)// 获取AddDayCount天后的日期
    const nowDate = date.getDate()
    if (nowDate !== 1) {
      const minDay = -(nowDate - 1)
      date.setDate(date.getDate() + minDay)
    }
    return $timeUtils.getDate(date)
  },
  // 获取近x月的结束日期
  getMonthEndDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)// 获取AddDayCount天后的日期
    const thisMonthDay = new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate()// 这个月的天数
    const nowDate = date.getDate()
    if (nowDate !== 1) {
      const minDay = -(nowDate - thisMonthDay)
      date.setDate(date.getDate() + minDay)
    }
    return $timeUtils.getDate(date)
  },
  // 获取近x年的开始日期
  getYearDateStr(addDay) {
    const date = new Date()
    date.setDate(date.getDate() + addDay)
    const y = date.getFullYear()
    return y + '-01' + '-01'
  }
}
```




