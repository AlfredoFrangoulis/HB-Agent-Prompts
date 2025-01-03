## Vue (JSX Basically 💀)

### INSTRUCTION
```
You are a professional translator specializing in Chinese and Vue source code translation. Your expertise focuses on Vue  code (.vue files) that primarily consist of the following three sections:
1. Template Section (<template>): Contains the HTML structure or markup for the component.
2. Script Section (<script>): Contains the component's logic, written in JavaScript or TypeScript (depending on the project configuration).
3. Style Section (<style>): Contains CSS or preprocessor code (e.g., SCSS, LESS, or Stylus) for styling the component.
   Your task is to accurately identify and translate Chinese text into English while ensuring that no other parts of the code are altered.

Follow the steps below carefully for Translation:
1. Analyze each line of each section of the Vue code to check for the presence of Chinese text.
2. For each identified Chinese text, record the following details in a structured format:
   a. lineNumber: The line number in the source code.
   b. lineType: Specify the type of line (e.g., Comment, Tag Value, Tag Attribute, Inline Script, or Other).
   c. jobType: Use the fixed text "Text Translation".
   d. originalText: The original Chinese text as it appears in the code.
   e. translatedText: The English translation corresponding to the original Chinese text.
3. Replace only the identified Chinese text fragments with their English translations in the original Vue code. Ensure the rest of the code remains intact, including formatting, logic, and any non-Chinese text.
4. Return the output as plain text having JSON object structure containing the following components:
   a. translatedCode: The full Vue source code with Chinese text replaced by its English translations.
   b. details: This is the record list we found in the step number 2.

Important Notes:
1. Translate the text exactly as it appears, without rephrasing or altering its meaning.
2. Do not include any Markdown formatting (e.g., ```json) in the response. Return plain text only.
3. Ensure that the formatting of the updated code matches the original file, and no structural or syntactical errors are introduced.
```

### Demonstration 1

#### Input:

```vue
<template>
  <div class="alarms" :class="'container_' + containerType">
    <div class="alarms-filter" :class="'transfrom_' + containerType">
      <div class="legends">
        <div v-for="(item, index) in legends" :key="index" class="legend">
          <span :class="['level_' + item.level, 'square']"></span>
          <span>{{ item.name }}</span>
        </div>
      </div>
      <i class="total">{{ t('common.total') }} {{ alarms.length }}</i>
      <div class="alarms-filter-option filter dark-class" :class="isFilter && 'filters'" @click="clickFilter">
        <img alt="filter" src="../imgs/filter.png"> {{ t('common.filter') }}
      </div>
      <div :class="['alarms-filter-option', 'filter', quick_filter === 1 && 'active']" @click="handleQuickFilter(1)"><span></span>{{ t('alarm.showUnConfirm') }}</div>
      <div :class="['alarms-filter-option', 'filter', quick_filter === 2 && 'active']" @click="handleQuickFilter(2)"><span></span>{{ t('alarm.showUnPlan') }}</div>
      <div :class="['alarms-filter-option', 'filter', quick_filter === 3 && 'active']" @click="handleQuickFilter(3)"><span></span>{{ t('alarm.showFollow') }}</div>

      <div v-click-outside="sortOutSide" class="alarms-filter-option sort" >
        <span @click="showSort = !showSort">
          <el-icon><Sort /></el-icon>
          {{ sortorderName || t('alarm.sortOrder') }}
        </span>
        <Transition name="slide-fade">
          <div v-if="showSort" class="slide">
            <div :class="['item', sortorder === 1 && 'active']" @click="handleSelectSort(1, t('alarm.recommendSort'))">{{ t('alarm.recommendSort') }}</div>
            <div :class="['item', sortorder === 2 && 'active']" @click="handleSelectSort(2, t('alarm.gradePriority'))">{{ t('alarm.gradePriority') }}</div>
            <div :class="['item', sortorder === 3 && 'active']" @click="handleSelectSort(3, t('alarm.timePriority'))">{{ t('alarm.timePriority') }}</div>
            <!-- <div :class="['item', sortorder === 4 && 'active']" @click="handleSelectSort(4)">确认优先</div> -->
            <div :class="['item', sortorder === 5 && 'active']" @click="handleSelectSort(5, t('alarm.focusPriority'))">{{ t('alarm.focusPriority') }}</div>
          </div>
        </Transition>
      </div>
    </div>
    <div v-if="alarms.length && !(differ.min === '00' && differ.second === '00')" class="alarms-time calculus">
      <span v-if="differ.min > '10'">
        >10 <i>{{ t('alarm.min') }}</i>
      </span>
      <span v-else>
        {{differ.min}}:{{differ.second}}
      </span>
    </div>
    <el-dropdown class="batch-dropdown">
      <span class="el-dropdown-link">
        <span v-if="hasAuthority('recovered.alarm.confirm.batch') || hasAuthority('unrecovered.alarm.confirm.batch') ||
          hasAuthority('message.alarm.confirm.batch')" class="alarms-batch cut-box">{{ t('alarm.batchClose') }}
          <img alt="" src="./../imgs/xiaodongxi.png" style="width:9px;position:relative;top: 1px;margin-left: 4px;"></span>
      </span>
      <template #dropdown>
        <el-dropdown-menu>
          <el-dropdown-item v-if="hasAuthority('message.alarm.confirm.batch')" @click="handleBatchClose('message')">{{ t('alarm.message')}}</el-dropdown-item>
          <el-dropdown-item v-if="hasAuthority('unrecovered.alarm.confirm.batch')" @click="handleBatchClose('unrecovered')">{{ t('alarm.unrecover')}}</el-dropdown-item>
          <el-dropdown-item v-if="hasAuthority('recovered.alarm.confirm.batch')" @click="handleBatchClose('recovered')">{{ t('alarm.recover')}}</el-dropdown-item>
        </el-dropdown-menu>
      </template>
    </el-dropdown>
    <span v-if="hasAuthority('alarm.notice')" class="alarms-notice cut-box" @click="handleNotice">{{ t('alarm.noticeProc') }}</span>
    <div :class="['alarms-view', model && 'view']" @click="clickModel"></div>
    <AlarmByItems v-if="!model" :alarms="alarms"></AlarmByItems>
    <AlarmByMain v-else :alarms="alarms" :sortorder="sortorder"></AlarmByMain>
  </div>
  <ConfirmModal v-if="showModal" :alarm_type="alarm_type" :mode="'batch'" :quick_filter="quick_filter" @close="closeModal"></ConfirmModal>
  <NoticeModal v-if="showNotice" @close="closeNotice"></NoticeModal>
</template>

<script setup lang="ts">
import { ref, inject, onActivated, computed } from 'vue'
import AlarmByMain from './AlarmByMain.vue'
import AlarmByItems from './AlarmByItems.vue'
import emitter from '@/util/mitt'
import { dayjs, ClickOutside as vClickOutside } from 'element-plus'
import ConfirmModal from './ConfirmModal.vue'
import NoticeModal from './notice/index.vue'
import { useI18n } from 'vue-i18n/dist/vue-i18n.cjs.js'
import { useStore } from 'vuex'
const store = useStore()
onActivated(() => {
  containerType.value = 0
})
const { t } = useI18n()
const hasAuthority: (arg: string, dc_id?: string | string[] | number) => boolean = inject('hasAuthority')

const model = ref(false)

// 图例
// eventLevel1: '一般',
//     eventLevel2: '重要',
//     eventLevel3: '严重',
//     eventLevel4: '紧急',
const legends = ref([{
  name: t('temp.eventLevel4'),
  level: 4
}, {
  name: t('temp.eventLevel3'),
  level: 3
}, {
  name: t('temp.eventLevel2'),
  level: 2
}, {
  name: t('temp.eventLevel1'),
  level: 1
}])
const containerType = ref(0) // 0:正常模式   1:高度减少模式  2:宽度减少模式   3:宽高都减少模式

// ----------------emitter
emitter.on('showFault', () => {
  if (containerType.value === 0) {
    containerType.value = 1
  } else if (containerType.value === 2) {
    containerType.value = 3
  }
})
emitter.on('hideFault', () => {
  if (containerType.value === 1) {
    containerType.value = 0
  } else if (containerType.value === 3) {
    containerType.value = 2
  }
})
emitter.on('hideContrast', () => {
  if (containerType.value === 2) {
    containerType.value = 0
  } else if (containerType.value === 3) {
    containerType.value = 1
  }
})
emitter.on('showContrast', () => {
  if (containerType.value === 0) {
    containerType.value = 2
  } else if (containerType.value === 1) {
    containerType.value = 3
  }
})

const isFilter = computed(() => {
  const filterParam = store.state.alarm.filterParam
  if (filterParam.selectDcs.length || filterParam.selectLevel.length || filterParam.selectMajor.length || filterParam.selectStatus.length ||
  filterParam.selectType.length || filterParam.startTime || filterParam.endTime || filterParam.selectCategory || filterParam.alarmName) {
    return true
  } else {
    return false
  }
})
const alarms = computed(() => {
  let alarms = store.state.alarm.alarmList.slice()
  const filterParam = store.state.alarm.filterParam
  if (filterParam.selectDcs.length) {
    alarms = alarms.filter(k => filterParam.selectDcs.includes(k.dcId))
  }
  if (filterParam.selectLevel.length) {
    alarms = alarms.filter(k => filterParam.selectLevel.includes(k.eventLevelId))
  }
  if (filterParam.selectMajor.length) {
    alarms = alarms.filter(k => filterParam.selectMajor.includes(k.majorId))
  }
  if (filterParam.selectType.length) {
    alarms = alarms.filter(k => filterParam.selectType.includes(k.eventTypeId))
  }
  if (filterParam.selectStatus.length) {
    alarms = alarms.filter(k => filterParam.selectStatus.includes(k.recoverFlag))
  }
  if (filterParam.startTime) {
    alarms = alarms.filter(k => dayjs(k.breakTime) > dayjs(filterParam.startTime))
  }
  if (filterParam.endTime) {
    alarms = alarms.filter(k => dayjs(k.breakTime) < dayjs(filterParam.endTime))
  }
  if (filterParam.selectCategory) {
    alarms = alarms.filter(k => k.eventTargetCategoryId === filterParam.selectCategory)
  }
  if (filterParam.selectMain) {
    alarms = alarms.filter(k => k.eventTargetCategoryId === filterParam.selectCategory && k.eventTargetInstId === filterParam.selectMain)
  }
  if (filterParam.alarmName) {
    alarms = alarms.filter(k => k.eventName.includes(filterParam.alarmName))
  }
  if (quick_filter.value === 1) {
    alarms = alarms.filter(k => !k.confirmFlag)
  } else if (quick_filter.value === 2) {
    alarms = alarms.filter(k => !k.planFlag)
  } else if (quick_filter.value === 3) {
    alarms = alarms.filter(k => k.keyFlag)
  }

  if (sortorder.value === 1) {
    alarms.sort((a, b) => {
      if (b.confirmFlag === a.confirmFlag) {
        if (b.autoNotificationFlag === a.autoNotificationFlag) {
          if (b.keyFlag === a.keyFlag) {
            if (b.eventLevelId === a.eventLevelId) {
              return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
            }
            return b.eventLevelId - a.eventLevelId
          }
          return b.keyFlag - a.keyFlag
        }
        return b.autoNotificationFlag - a.autoNotificationFlag
      }
      return a.confirmFlag - b.confirmFlag
    })
  } else if (sortorder.value === 2) {
    alarms.sort((a, b) => {
      if (b.eventLevelId === a.eventLevelId) {
        return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
      }
      return b.eventLevelId - a.eventLevelId
    })
  } else if (sortorder.value === 3) {
    alarms.sort((a, b) => {
      return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
    })
  } else if (sortorder.value === 4) {
    alarms.sort((a, b) => {
      if (b.confirmFlag === a.confirmFlag) {
        if (b.eventLevelId === a.eventLevelId) {
          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
        }
        return b.eventLevelId - a.eventLevelId
      }
      return a.confirmFlag - b.confirmFlag
    })
  } else if (sortorder.value === 5) {
    alarms.sort((a, b) => {
      if (b.keyFlag === a.keyFlag) {
        if (b.eventLevelId === a.eventLevelId) {
          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
        }
        return b.eventLevelId - a.eventLevelId
      }
      return b.keyFlag - a.keyFlag
    })
  }
  return alarms
})

// -----------------------------------------------------------  过滤相关  -------------------------------------------------------
const quick_filter = ref(null)
const handleQuickFilter = (val) => {
  store.commit('SET_GUIDE', false)
  if (quick_filter.value === val) {
    quick_filter.value = null
  } else {
    quick_filter.value = val
  }
}
const clickFilter = () => {
  store.commit('SET_GUIDE', false)
  emitter.emit('showFilter')
}

// -----------------------------------------------------------  倒计时相关  -------------------------------------------------------
const timer = ref(null)
const differ = computed(() => {
  clearInterval(timer.value)
  // eslint-disable-next-line vue/no-side-effects-in-computed-properties
timer.value = null
  const activeAlarm = alarms.value.filter(item => !item.planFlag && !item.confirmFlag && !item.recoverFlag)

  if (activeAlarm.length) {
    const { min, second } = formatTime(activeAlarm)
    return {
      min: min,
      second: second
    }
  } else {
    clearInterval(timer.value)
    // eslint-disable-next-line vue/no-side-effects-in-computed-properties
timer.value = null
    return { min: '00', second: '00' }
  }
})
const checkTime = (i) => {
  // 将0-9的数字前面加上0，例1变为01
  if (i < 10) {
    i = '0' + i
  }
  return i
}
const formatTime = (activeAlarm) => {
  const min_time = activeAlarm.sort((a, b) => {
    return new Date(a.updateTime).getTime() - new Date(b.updateTime).getTime()
  })[0].updateTime
  const diff = dayjs(min_time).diff(dayjs(new Date()), 'seconds')
  const second = checkTime(parseInt(Math.abs(diff) % 60 + ''))
  const min = checkTime(parseInt((Math.abs(diff) / 60) + ''))
  return { min, second }
}

// -----------------------------------------------------------  排序相关  -------------------------------------------------------
const showSort = ref(false)
const sortorderName = ref(t('alarm.recommendSort'))
// 1 推荐排序：未确认 > 要自动通知的 > 关注 > 等级倒序(高等级在前) > 更新时间倒序
// 2 等级优先：等级倒序 > 更新时间倒序
// 3 时间优先：更新时间倒序
// 4 确认优先：未确认 > 等级倒序 > 更新时间倒序
// 5 关注优先：关注 > 等级倒序 > 更新时间倒序
const sortorder = ref<1|2|3|4|5>(1)
const sortOutSide = () => {
  showSort.value = false
}
const handleSelectSort = (int, name) => {
  sortorder.value = int
  sortorderName.value = name
  sortAlarm()
  showSort.value = false
}
// 告警排序
const sortAlarm = () => {
  if (sortorder.value === 1) {
    alarms.value.sort((a, b) => {
      if (b.confirmFlag === a.confirmFlag) {
        if (b.autoNotificationFlag === a.autoNotificationFlag) {
          if (b.keyFlag === a.keyFlag) {
            if (b.eventLevelId === a.eventLevelId) {
              return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
            }
            return b.eventLevelId - a.eventLevelId
          }
          return b.keyFlag - a.keyFlag
        }
        return b.autoNotificationFlag - a.autoNotificationFlag
      }
      return a.confirmFlag - b.confirmFlag
    })
  } else if (sortorder.value === 2) {
    alarms.value.sort((a, b) => {
      if (b.eventLevelId === a.eventLevelId) {
        return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
      }
      return b.eventLevelId - a.eventLevelId
    })
  } else if (sortorder.value === 3) {
    alarms.value.sort((a, b) => {
      return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
    })
  } else if (sortorder.value === 4) {
    alarms.value.sort((a, b) => {
      if (b.confirmFlag === a.confirmFlag) {
        if (b.eventLevelId === a.eventLevelId) {
          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
        }
        return b.eventLevelId - a.eventLevelId
      }
      return a.confirmFlag - b.confirmFlag
    })
  } else if (sortorder.value === 5) {
    alarms.value.sort((a, b) => {
      if (b.keyFlag === a.keyFlag) {
        if (b.eventLevelId === a.eventLevelId) {
          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()
        }
        return b.eventLevelId - a.eventLevelId
      }
      return b.keyFlag - a.keyFlag
    })
  }
}

// ========================== 批量关闭
const showModal = ref(false)
const alarm_type = ref('')
const closeModal = () => {
  showModal.value = false
}

const handleBatchClose = (type) => {
  alarm_type.value = type
  showModal.value = true
}

// ================================  通知更新
const showNotice = ref(false)
const closeNotice = () => {
  showNotice.value = false
}
const handleNotice = () => {
  showNotice.value = true
}

// =======切换视图
const clickModel = () => {
  model.value = !model.value
  store.commit('SET_ALARM_ID', null)
  store.commit('SET_GUIDE', false)
}
</script>

<style scoped lang="stylus">
@import '../styles/var.styl';
.test-item
  height 135px
  border 1px solid #000
  position absolute

.dark
  .alarms
    background rgba(8, 12, 17, 1)
    border 1px solid RGBA(45, 123, 149, 1)

    &-filter
      .legends
        color rgba(145, 213, 223, 1)

    &-container
      &::-webkit-scrollbar-track
        background rgba(45, 123, 149, .2)

      &::-webkit-scrollbar-thumb
        background rgba(45, 123, 149, 1)

    .total
      color #43affd

    .dark-class
      color #43affd !important

    .filter
      color RGBA(113, 166, 175, 1)

      span
        background rgba(8, 12, 17, 1)
        border 1px solid RGBA(40, 55, 73, 1)

        &:before
          background RGBA(62, 83, 110, 1)
      &.active
        span
          border-color RGBA(90, 255, 195, 1)

          &:before
            left 8px
            background RGBA(90, 255, 195, 1)

    &-notice, .batch-dropdown .alarms-batch
      background transparent

    &-time
      background url('./../imgs/time_bg_dark.png') no-repeat
      background-size 100% 100%
      color rgba(45, 123, 149, 1)

    &-view
      background url('./../imgs/view1_dark.png') no-repeat
      background-size 100% 100%
      &.view
        background url('./../imgs/view2_dark.png') no-repeat
        background-size 100% 100%
.alarms
  background #fff
  padding 0 6px 10px 6px
  position relative
  width calc(100% - 75px)
  height calc(100% - 125px)
  transition all .3s
  border 1px solid #fff

  &.container_1
    height calc(100% - 416px)
  &.container_2
    width calc(100% - 575px)
  &.container_3
    width calc(100% - 575px)
    height calc(100% - 416px)

  .total
    font-style normal
    position absolute
    left 20px
    top 10px

  &-filter
    display flex
    padding 30px 0 46px 300px
    transition all .3s
    position relative

    .legends
      position absolute
      bottom 12px
      left 120px
      display flex
      .legend
        display flex
        margin-right 20px
        align-items center
        font-size 13px
      .square
        width 18px
        height 4px
        background green
        display inline-block
        clip-path polygon(0% 0, 100% 0%, 80% 100%, 0% 100%);
        margin-right 5px

        &.level_4
          background $color-level4
        &.level_3
          background $color-level3
        &.level_2
          background $color-level2
        &.level_1
          background $color-level1

    &.transfrom_3, &.transfrom_2
      padding 30px 0 46px 30px

    &-option
      margin 0 20px
      color rgba(68, 68, 68, 1)
      font-size 14px
      cursor pointer

      &.filter
        position relative

        &.filters

          &:after
            content ''
            position: absolute;
            background: #ff1a1a;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            top: 0px;
            right: -1px;

        &.active
          span
            border-color rgba(67, 175, 253, 1)

            &:before
              left 8px
              background rgba(67, 175, 253, 1)

        img
          width 19px
          position relative
          top 2px
          margin-left 18px

        span
          width 20px
          display inline-block
          height 6px
          background #FFFFFF
          border 1px solid rgba(170, 170, 170, 1)
          border-radius 3px
          position relative
          margin-right 7px
          transition all .3s

          &:before
            content ''
            width 12px
            height 10px
            background rgba(187, 187, 187, 1)
            border-radius 3px
            position absolute
            top -2px
            left 0
            transition all .3s

      &.sort
        color rgba(67, 175, 253, 1)
        position relative

        .slide
          top -8px
          right -110px

          .item.active
            color #43affd

        .el-icon
          position relative
          top 2px
  &-notice
    position absolute
    top 30px
    font-size 12px
    padding 0 12px
    line-height 24px
    cursor pointer
    color #43affd
    border 1px solid #43affd
    background #fff

  .batch-dropdown
    position absolute
    top 30px
    right 280px

    .alarms-batch
      font-size 12px
      padding 0 12px
      line-height 24px
      cursor pointer
      color #43affd
      border 1px solid #43affd
      background #fff
      display block

  &-notice
    right 195px
  &-view
    position absolute
    background url('./../imgs/view1.png') no-repeat
    background-size 100% 100%
    width 87px
    height 24px
    right 95px
    top 30px
    cursor pointer

    &.view
      background url('./../imgs/view2.png') no-repeat
      background-size 100% 100%

  &-time
    position absolute
    background url('./../imgs/time_bg.png') no-repeat
    background-size 100% 100%
    right 0
    top 0
    width 83px
    height 31px
    color rgba(51, 51, 51, 1)
    font-size 22px
    line-height 31px
    padding-left 23px

    i
      font-style normal
      font-size 12px
      position relative
      top -2px
      left -2px

  &-container
    display flex
    align-content flex-start
    flex-flow row wrap
    justify-content center
    overflow-y scroll
    position relative
    height calc(100% - 90px)
    padding-top 1px

    &::-webkit-scrollbar
      width 4px
      height 4px

    &::-webkit-scrollbar-track
      background rgba(220, 220, 220, 1)
      border-radius 2px

    &::-webkit-scrollbar-thumb
      background rgba(144, 201, 243, 1)
      border-radius 10px

    > i
      width 450px
      margin-right 18px
</style>
```

#### Output:

```json
{
  "translatedCode": "<template>\n  <div class=\"alarms\" :class=\"'container_' + containerType\">\n    <div class=\"alarms-filter\" :class=\"'transfrom_' + containerType\">\n      <div class=\"legends\">\n        <div v-for=\"(item, index) in legends\" :key=\"index\" class=\"legend\">\n          <span :class=\"['level_' + item.level, 'square']\"></span>\n          <span>{{ item.name }}</span>\n        </div>\n      </div>\n      <i class=\"total\">{{ t('common.total') }} {{ alarms.length }}</i>\n      <div class=\"alarms-filter-option filter dark-class\" :class=\"isFilter && 'filters'\" @click=\"clickFilter\">\n        <img alt=\"filter\" src=\"../imgs/filter.png\"> {{ t('common.filter') }}\n      </div>\n      <div :class=\"['alarms-filter-option', 'filter', quick_filter === 1 && 'active']\" @click=\"handleQuickFilter(1)\"><span></span>{{ t('alarm.showUnConfirm') }}</div>\n      <div :class=\"['alarms-filter-option', 'filter', quick_filter === 2 && 'active']\" @click=\"handleQuickFilter(2)\"><span></span>{{ t('alarm.showUnPlan') }}</div>\n      <div :class=\"['alarms-filter-option', 'filter', quick_filter === 3 && 'active']\" @click=\"handleQuickFilter(3)\"><span></span>{{ t('alarm.showFollow') }}</div>\n\n      <div v-click-outside=\"sortOutSide\" class=\"alarms-filter-option sort\" >\n        <span @click=\"showSort = !showSort\">\n          <el-icon><Sort /></el-icon>\n          {{ sortorderName || t('alarm.sortOrder') }}\n        </span>\n        <Transition name=\"slide-fade\">\n          <div v-if=\"showSort\" class=\"slide\">\n            <div :class=\"['item', sortorder === 1 && 'active']\" @click=\"handleSelectSort(1, t('alarm.recommendSort'))\">{{ t('alarm.recommendSort') }}</div>\n            <div :class=\"['item', sortorder === 2 && 'active']\" @click=\"handleSelectSort(2, t('alarm.gradePriority'))\">{{ t('alarm.gradePriority') }}</div>\n            <div :class=\"['item', sortorder === 3 && 'active']\" @click=\"handleSelectSort(3, t('alarm.timePriority'))\">{{ t('alarm.timePriority') }}</div>\n            <!-- <div :class=\"['item', sortorder === 4 && 'active']\" @click=\"handleSelectSort(4)\">Confirm Priority</div> -->\n            <div :class=\"['item', sortorder === 5 && 'active']\" @click=\"handleSelectSort(5, t('alarm.focusPriority'))\">{{ t('alarm.focusPriority') }}</div>\n          </div>\n        </Transition>\n      </div>\n    </div>\n    <div v-if=\"alarms.length && !(differ.min === '00' && differ.second === '00')\" class=\"alarms-time calculus\">\n      <span v-if=\"differ.min > '10'\">\n        >10 <i>{{ t('alarm.min') }}</i>\n      </span>\n      <span v-else>\n        {{differ.min}}:{{differ.second}}\n      </span>\n    </div>\n    <el-dropdown class=\"batch-dropdown\">\n      <span class=\"el-dropdown-link\">\n        <span v-if=\"hasAuthority('recovered.alarm.confirm.batch') || hasAuthority('unrecovered.alarm.confirm.batch') ||\n          hasAuthority('message.alarm.confirm.batch')\" class=\"alarms-batch cut-box\">{{ t('alarm.batchClose') }}\n          <img alt=\"\" src=\"./../imgs/xiaodongxi.png\" style=\"width:9px;position:relative;top: 1px;margin-left: 4px;\"></span>\n      </span>\n      <template #dropdown>\n        <el-dropdown-menu>\n          <el-dropdown-item v-if=\"hasAuthority('message.alarm.confirm.batch')\" @click=\"handleBatchClose('message')\">{{ t('alarm.message')}}</el-dropdown-item>\n          <el-dropdown-item v-if=\"hasAuthority('unrecovered.alarm.confirm.batch')\" @click=\"handleBatchClose('unrecovered')\">{{ t('alarm.unrecover')}}</el-dropdown-item>\n          <el-dropdown-item v-if=\"hasAuthority('recovered.alarm.confirm.batch')\" @click=\"handleBatchClose('recovered')\">{{ t('alarm.recover')}}</el-dropdown-item>\n        </el-dropdown-menu>\n      </template>\n    </el-dropdown>\n    <span v-if=\"hasAuthority('alarm.notice')\" class=\"alarms-notice cut-box\" @click=\"handleNotice\">{{ t('alarm.noticeProc') }}</span>\n    <div :class=\"['alarms-view', model && 'view']\" @click=\"clickModel\"></div>\n    <AlarmByItems v-if=\"!model\" :alarms=\"alarms\"></AlarmByItems>\n    <AlarmByMain v-else :alarms=\"alarms\" :sortorder=\"sortorder\"></AlarmByMain>\n  </div>\n  <ConfirmModal v-if=\"showModal\" :alarm_type=\"alarm_type\" :mode=\"'batch'\" :quick_filter=\"quick_filter\" @close=\"closeModal\"></ConfirmModal>\n  <NoticeModal v-if=\"showNotice\" @close=\"closeNotice\"></NoticeModal>\n</template>\n\n<script setup lang=\"ts\">\nimport { ref, inject, onActivated, computed } from 'vue'\nimport AlarmByMain from './AlarmByMain.vue'\nimport AlarmByItems from './AlarmByItems.vue'\nimport emitter from '@/util/mitt'\nimport { dayjs, ClickOutside as vClickOutside } from 'element-plus'\nimport ConfirmModal from './ConfirmModal.vue'\nimport NoticeModal from './notice/index.vue'\nimport { useI18n } from 'vue-i18n/dist/vue-i18n.cjs.js'\nimport { useStore } from 'vuex'\nconst store = useStore()\nonActivated(() => {\n  containerType.value = 0\n})\nconst { t } = useI18n()\nconst hasAuthority: (arg: string, dc_id?: string | string[] | number) => boolean = inject('hasAuthority')\n\nconst model = ref(false)\n\n// Legend\n// eventLevel1: 'General',\n//     eventLevel2: 'Important',\n//     eventLevel3: 'Serious',\n//     eventLevel4: 'Urgent',\nconst legends = ref([{\n  name: t('temp.eventLevel4'),\n  level: 4\n}, {\n  name: t('temp.eventLevel3'),\n  level: 3\n}, {\n  name: t('temp.eventLevel2'),\n  level: 2\n}, {\n  name: t('temp.eventLevel1'),\n  level: 1\n}])\nconst containerType = ref(0) // 0:Normal mode   1:Height reduction mode  2:Width reduction mode   3:Both width and height reduction mode\n\n// ----------------emitter\nemitter.on('showFault', () => {\n  if (containerType.value === 0) {\n    containerType.value = 1\n  } else if (containerType.value === 2) {\n    containerType.value = 3\n  }\n})\nemitter.on('hideFault', () => {\n  if (containerType.value === 1) {\n    containerType.value = 0\n  } else if (containerType.value === 3) {\n    containerType.value = 2\n  }\n})\nemitter.on('hideContrast', () => {\n  if (containerType.value === 2) {\n    containerType.value = 0\n  } else if (containerType.value === 3) {\n    containerType.value = 1\n  }\n})\nemitter.on('showContrast', () => {\n  if (containerType.value === 0) {\n    containerType.value = 2\n  } else if (containerType.value === 1) {\n    containerType.value = 3\n  }\n})\n\nconst isFilter = computed(() => {\n  const filterParam = store.state.alarm.filterParam\n  if (filterParam.selectDcs.length || filterParam.selectLevel.length || filterParam.selectMajor.length || filterParam.selectStatus.length ||\n  filterParam.selectType.length || filterParam.startTime || filterParam.endTime || filterParam.selectCategory || filterParam.alarmName) {\n    return true\n  } else {\n    return false\n  }\n})\nconst alarms = computed(() => {\n  let alarms = store.state.alarm.alarmList.slice()\n  const filterParam = store.state.alarm.filterParam\n  if (filterParam.selectDcs.length) {\n    alarms = alarms.filter(k => filterParam.selectDcs.includes(k.dcId))\n  }\n  if (filterParam.selectLevel.length) {\n    alarms = alarms.filter(k => filterParam.selectLevel.includes(k.eventLevelId))\n  }\n  if (filterParam.selectMajor.length) {\n    alarms = alarms.filter(k => filterParam.selectMajor.includes(k.majorId))\n  }\n  if (filterParam.selectType.length) {\n    alarms = alarms.filter(k => filterParam.selectType.includes(k.eventTypeId))\n  }\n  if (filterParam.selectStatus.length) {\n    alarms = alarms.filter(k => filterParam.selectStatus.includes(k.recoverFlag))\n  }\n  if (filterParam.startTime) {\n    alarms = alarms.filter(k => dayjs(k.breakTime) > dayjs(filterParam.startTime))\n  }\n  if (filterParam.endTime) {\n    alarms = alarms.filter(k => dayjs(k.breakTime) < dayjs(filterParam.endTime))\n  }\n  if (filterParam.selectCategory) {\n    alarms = alarms.filter(k => k.eventTargetCategoryId === filterParam.selectCategory)\n  }\n  if (filterParam.selectMain) {\n    alarms = alarms.filter(k => k.eventTargetCategoryId === filterParam.selectCategory && k.eventTargetInstId === filterParam.selectMain)\n  }\n  if (filterParam.alarmName) {\n    alarms = alarms.filter(k => k.eventName.includes(filterParam.alarmName))\n  }\n  if (quick_filter.value === 1) {\n    alarms = alarms.filter(k => !k.confirmFlag)\n  } else if (quick_filter.value === 2) {\n    alarms = alarms.filter(k => !k.planFlag)\n  } else if (quick_filter.value === 3) {\n    alarms = alarms.filter(k => k.keyFlag)\n  }\n\n  if (sortorder.value === 1) {\n    alarms.sort((a, b) => {\n      if (b.confirmFlag === a.confirmFlag) {\n        if (b.autoNotificationFlag === a.autoNotificationFlag) {\n          if (b.keyFlag === a.keyFlag) {\n            if (b.eventLevelId === a.eventLevelId) {\n              return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n            }\n            return b.eventLevelId - a.eventLevelId\n          }\n          return b.keyFlag - a.keyFlag\n        }\n        return b.autoNotificationFlag - a.autoNotificationFlag\n      }\n      return a.confirmFlag - b.confirmFlag\n    })\n  } else if (sortorder.value === 2) {\n    alarms.sort((a, b) => {\n      if (b.eventLevelId === a.eventLevelId) {\n        return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n      }\n      return b.eventLevelId - a.eventLevelId\n    })\n  } else if (sortorder.value === 3) {\n    alarms.sort((a, b) => {\n      return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n    })\n  } else if (sortorder.value === 4) {\n    alarms.sort((a, b) => {\n      if (b.confirmFlag === a.confirmFlag) {\n        if (b.eventLevelId === a.eventLevelId) {\n          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n        }\n        return b.eventLevelId - a.eventLevelId\n      }\n      return a.confirmFlag - b.confirmFlag\n    })\n  } else if (sortorder.value === 5) {\n    alarms.sort((a, b) => {\n      if (b.keyFlag === a.keyFlag) {\n        if (b.eventLevelId === a.eventLevelId) {\n          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n        }\n        return b.eventLevelId - a.eventLevelId\n      }\n      return b.keyFlag - a.keyFlag\n    })\n  }\n  return alarms\n})\n\n// -----------------------------------------------------------  Filter related  -------------------------------------------------------\nconst quick_filter = ref(null)\nconst handleQuickFilter = (val) => {\n  store.commit('SET_GUIDE', false)\n  if (quick_filter.value === val) {\n    quick_filter.value = null\n  } else {\n    quick_filter.value = val\n  }\n}\nconst clickFilter = () => {\n  store.commit('SET_GUIDE', false)\n  emitter.emit('showFilter')\n}\n\n// -----------------------------------------------------------  Countdown related  -------------------------------------------------------\nconst timer = ref(null)\nconst differ = computed(() => {\n  clearInterval(timer.value)\n  // eslint-disable-next-line vue/no-side-effects-in-computed-properties\ntimer.value = null\n  const activeAlarm = alarms.value.filter(item => !item.planFlag && !item.confirmFlag && !item.recoverFlag)\n\n  if (activeAlarm.length) {\n    const { min, second } = formatTime(activeAlarm)\n    return {\n      min: min,\n      second: second\n    }\n  } else {\n    clearInterval(timer.value)\n    // eslint-disable-next-line vue/no-side-effects-in-computed-properties\ntimer.value = null\n    return { min: '00', second: '00' }\n  }\n})\nconst checkTime = (i) => {\n  // Adds a 0 in front of numbers 0-9, e.g., 1 becomes 01\n  if (i < 10) {\n    i = '0' + i\n  }\n  return i\n}\nconst formatTime = (activeAlarm) => {\n  const min_time = activeAlarm.sort((a, b) => {\n    return new Date(a.updateTime).getTime() - new Date(b.updateTime).getTime()\n  })[0].updateTime\n  const diff = dayjs(min_time).diff(dayjs(new Date()), 'seconds')\n  const second = checkTime(parseInt(Math.abs(diff) % 60 + ''))\n  const min = checkTime(parseInt((Math.abs(diff) / 60) + ''))\n  return { min, second }\n}\n\n// -----------------------------------------------------------  Sorting related  -------------------------------------------------------\nconst showSort = ref(false)\nconst sortorderName = ref(t('alarm.recommendSort'))\n// 1 Recommended sorting: Unconfirmed > To be automatically notified > Followed > Level (high level first) > Update time\n// 2 Level priority: Level > Update time\n// 3 Time priority: Update time\n// 4 Confirm priority: Unconfirmed > Level > Update time\n// 5 Follow priority: Followed > Level > Update time\nconst sortorder = ref<1|2|3|4|5>(1)\nconst sortOutSide = () => {\n  showSort.value = false\n}\nconst handleSelectSort = (int, name) => {\n  sortorder.value = int\n  sortorderName.value = name\n  sortAlarm()\n  showSort.value = false\n}\n// Alarm sorting\nconst sortAlarm = () => {\n  if (sortorder.value === 1) {\n    alarms.value.sort((a, b) => {\n      if (b.confirmFlag === a.confirmFlag) {\n        if (b.autoNotificationFlag === a.autoNotificationFlag) {\n          if (b.keyFlag === a.keyFlag) {\n            if (b.eventLevelId === a.eventLevelId) {\n              return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n            }\n            return b.eventLevelId - a.eventLevelId\n          }\n          return b.keyFlag - a.keyFlag\n        }\n        return b.autoNotificationFlag - a.autoNotificationFlag\n      }\n      return a.confirmFlag - b.confirmFlag\n    })\n  } else if (sortorder.value === 2) {\n    alarms.value.sort((a, b) => {\n      if (b.eventLevelId === a.eventLevelId) {\n        return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n      }\n      return b.eventLevelId - a.eventLevelId\n    })\n  } else if (sortorder.value === 3) {\n    alarms.value.sort((a, b) => {\n      return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n    })\n  } else if (sortorder.value === 4) {\n    alarms.value.sort((a, b) => {\n      if (b.confirmFlag === a.confirmFlag) {\n        if (b.eventLevelId === a.eventLevelId) {\n          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n        }\n        return b.eventLevelId - a.eventLevelId\n      }\n      return a.confirmFlag - b.confirmFlag\n    })\n  } else if (sortorder.value === 5) {\n    alarms.value.sort((a, b) => {\n      if (b.keyFlag === a.keyFlag) {\n        if (b.eventLevelId === a.eventLevelId) {\n          return new Date(b.updateTime).getTime() - new Date(a.updateTime).getTime()\n        }\n        return b.eventLevelId - a.eventLevelId\n      }\n      return b.keyFlag - a.keyFlag\n    })\n  }\n}\n\n// ========================== Batch close\nconst showModal = ref(false)\nconst alarm_type = ref('')\nconst closeModal = () => {\n  showModal.value = false\n}\n\nconst handleBatchClose = (type) => {\n  alarm_type.value = type\n  showModal.value = true\n}\n\n// ================================  Notice update\nconst showNotice = ref(false)\nconst closeNotice = () => {\n  showNotice.value = false\n}\nconst handleNotice = () => {\n  showNotice.value = true\n}\n\n// =======Switch view\nconst clickModel = () => {\n  model.value = !model.value\n  store.commit('SET_ALARM_ID', null)\n  store.commit('SET_GUIDE', false)\n}\n</script>\n\n<style scoped lang=\"stylus\">\n@import '../styles/var.styl';\n.test-item\n  height 135px\n  border 1px solid #000\n  position absolute\n\n.dark\n  .alarms\n    background rgba(8, 12, 17, 1)\n    border 1px solid RGBA(45, 123, 149, 1)\n\n    &-filter\n      .legends\n        color rgba(145, 213, 223, 1)\n\n    &-container\n      &::-webkit-scrollbar-track\n        background rgba(45, 123, 149, .2)\n\n      &::-webkit-scrollbar-thumb\n        background rgba(45, 123, 149, 1)\n\n    .total\n      color #43affd\n\n    .dark-class\n      color #43affd !important\n\n    .filter\n      color RGBA(113, 166, 175, 1)\n\n      span\n        background rgba(8, 12, 17, 1)\n        border 1px solid RGBA(40, 55, 73, 1)\n\n        &:before\n          background RGBA(62, 83, 110, 1)\n      &.active\n        span\n          border-color RGBA(90, 255, 195, 1)\n\n          &:before\n            left 8px\n            background RGBA(90, 255, 195, 1)\n\n    &-notice, .batch-dropdown .alarms-batch\n      background transparent\n\n    &-time\n      background url('./../imgs/time_bg_dark.png') no-repeat\n      background-size 100% 100%\n      color rgba(45, 123, 149, 1)\n\n    &-view\n      background url('./../imgs/view1_dark.png') no-repeat\n      background-size 100% 100%\n      &.view\n        background url('./../imgs/view2_dark.png') no-repeat\n        background-size 100% 100%\n.alarms\n  background #fff\n  padding 0 6px 10px 6px\n  position relative\n  width calc(100% - 75px)\n  height calc(100% - 125px)\n  transition all .3s\n  border 1px solid #fff\n\n  &.container_1\n    height calc(100% - 416px)\n  &.container_2\n    width calc(100% - 575px)\n  &.container_3\n    width calc(100% - 575px)\n    height calc(100% - 416px)\n\n  .total\n    font-style normal\n    position absolute\n    left 20px\n    top 10px\n\n  &-filter\n    display flex\n    padding 30px 0 46px 300px\n    transition all .3s\n    position relative\n\n    .legends\n      position absolute\n      bottom 12px\n      left 120px\n      display flex\n      .legend\n        display flex\n        margin-right 20px\n        align-items center\n        font-size 13px\n      .square\n        width 18px\n        height 4px\n        background green\n        display inline-block\n        clip-path polygon(0% 0, 100% 0%, 80% 100%, 0% 100%);\n        margin-right 5px\n\n        &.level_4\n          background $color-level4\n        &.level_3\n          background $color-level3\n        &.level_2\n          background $color-level2\n        &.level_1\n          background $color-level1\n\n    &.transfrom_3, &.transfrom_2\n      padding 30px 0 46px 30px\n\n    &-option\n      margin 0 20px\n      color rgba(68, 68, 68, 1)\n      font-size 14px\n      cursor pointer\n\n      &.filter\n        position relative\n\n        &.filters\n\n          &:after\n            content ''\n            position: absolute;\n            background: #ff1a1a;\n            width: 6px;\n            height: 6px;\n            border-radius: 50%;\n            top: 0px;\n            right: -1px;\n\n        &.active\n          span\n            border-color rgba(67, 175, 253, 1)\n\n            &:before\n              left 8px\n              background rgba(67, 175, 253, 1)\n\n        img\n          width 19px\n          position relative\n          top 2px\n          margin-left 18px\n\n        span\n          width 20px\n          display inline-block\n          height 6px\n          background #FFFFFF\n          border 1px solid rgba(170, 170, 170, 1)\n          border-radius 3px\n          position relative\n          margin-right 7px\n          transition all .3s\n\n          &:before\n            content ''\n            width 12px\n            height 10px\n            background rgba(187, 187, 187, 1)\n            border-radius 3px\n            position absolute\n            top -2px\n            left 0\n            transition all .3s\n\n      &.sort\n        color rgba(67, 175, 253, 1)\n        position relative\n\n        .slide\n          top -8px\n          right -110px\n\n          .item.active\n            color #43affd\n\n        .el-icon\n          position relative\n          top 2px\n  &-notice\n    position absolute\n    top 30px\n    font-size 12px\n    padding 0 12px\n    line-height 24px\n    cursor pointer\n    color #43affd\n    border 1px solid #43affd\n    background #fff\n\n  .batch-dropdown\n    position absolute\n    top 30px\n    right 280px\n\n    .alarms-batch\n      font-size 12px\n      padding 0 12px\n      line-height 24px\n      cursor pointer\n      color #43affd\n      border 1px solid #43affd\n      background #fff\n      display block\n\n  &-notice\n    right 195px\n  &-view\n    position absolute\n    background url('./../imgs/view1.png') no-repeat\n    background-size 100% 100%\n    width 87px\n    height 24px\n    right 95px\n    top 30px\n    cursor pointer\n\n    &.view\n      background url('./../imgs/view2.png') no-repeat\n      background-size 100% 100%\n\n  &-time\n    position absolute\n    background url('./../imgs/time_bg.png') no-repeat\n    background-size 100% 100%\n    right 0\n    top 0\n    width 83px\n    height 31px\n    color rgba(51, 51, 51, 1)\n    font-size 22px\n    line-height 31px\n    padding-left 23px\n\n    i\n      font-style normal\n      font-size 12px\n      position relative\n      top -2px\n      left -2px\n\n  &-container\n    display flex\n    align-content flex-start\n    flex-flow row wrap\n    justify-content center\n    overflow-y scroll\n    position relative\n    height calc(100% - 90px)\n    padding-top 1px\n\n    &::-webkit-scrollbar\n      width 4px\n      height 4px\n\n    &::-webkit-scrollbar-track\n      background rgba(220, 220, 220, 1)\n      border-radius 2px\n\n    &::-webkit-scrollbar-thumb\n      background rgba(144, 201, 243, 1)\n      border-radius 10px\n\n    > i\n      width 450px\n      margin-right 18px\n</style>",
  "details": [
    {
      "lineNumber": 28,
      "lineType": "Tag Value",
      "jobType": "Text Translation",
      "originalText": "确认优先",
      "translatedText": "Confirm Priority"
    },
    {
      "lineNumber": 84,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "图例",
      "translatedText": "Legend"
    },
    {
      "lineNumber": 85,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "一般",
      "translatedText": "General"
    },
    {
      "lineNumber": 86,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "重要",
      "translatedText": "Important"
    },
    {
      "lineNumber": 87,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "严重",
      "translatedText": "Serious"
    },
    {
      "lineNumber": 88,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "紧急",
      "translatedText": "Urgent"
    },
    {
      "lineNumber": 102,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "0:正常模式   1:高度减少模式  2:宽度减少模式   3:宽高都减少模式",
      "translatedText": "0:Normal mode   1:Height reduction mode  2:Width reduction mode   3:Both width and height reduction mode"
    },
    {
      "lineNumber": 235,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "过滤相关",
      "translatedText": "Filter related"
    },
    {
      "lineNumber": 250,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "倒计时相关",
      "translatedText": "Countdown related"
    },
    {
      "lineNumber": 272,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "将0-9的数字前面加上0，例1变为01",
      "translatedText": "Adds a 0 in front of numbers 0-9, e.g., 1 becomes 01"
    },
    {
      "lineNumber": 288,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "排序相关",
      "translatedText": "Sorting related"
    },
    {
      "lineNumber": 291,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "推荐排序：未确认 > 要自动通知的 > 关注 > 等级倒序(高等级在前) > 更新时间倒序",
      "translatedText": "Recommended sorting: Unconfirmed > To be automatically notified > Followed > Level (high level first) > Update time"
    },
    {
      "lineNumber": 292,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "等级优先：等级倒序 > 更新时间倒序",
      "translatedText": "Level priority: Level > Update time"
    },
    {
      "lineNumber": 293,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "时间优先：更新时间倒序",
      "translatedText": "Time priority: Update time"
    },
    {
      "lineNumber": 294,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "确认优先：未确认 > 等级倒序 > 更新时间倒序",
      "translatedText": "Confirm priority: Unconfirmed > Level > Update time"
    },
    {
      "lineNumber": 295,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "关注优先：关注 > 等级倒序 > 更新时间倒序",
      "translatedText": "Follow priority: Followed > Level > Update time"
    },
    {
      "lineNumber": 306,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "告警排序",
      "translatedText": "Alarm sorting"
    },
    {
      "lineNumber": 358,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "批量关闭",
      "translatedText": "Batch close"
    },
    {
      "lineNumber": 370,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "通知更新",
      "translatedText": "Notice update"
    },
    {
      "lineNumber": 379,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "切换视图",
      "translatedText": "Switch view"
    }
  ]
}
```

#### Input:

```vue
<template>
  <div v-loading="loading" class="temp">
    <!-- 条件选择 -->
    <el-collapse-transition>
      <section v-show="showFilter" class="filter" >
      <el-row :gutter="5">
        <el-col :span="6">
          <div class="item">
            <label>{{ t('alarm.dataCenter') }}：</label>
            <el-select v-model="dc_ids" clearable collapse-tags collapse-tags-tooltip filterable multiple
              :placeholder="t('common.pleaseSelect')"
              @change="handleChange">
              <el-option v-for="item in dcs" :key="item.dc_id" :label="item.dc_name" :value="item.dc_id"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('alarm.classification') }}：</label>
            <el-select v-model="classification" clearable collapse-tags multiple :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in classifications" :key="'status_' + item.code" :label="item.name" :value="item.code"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('common.state') }}：</label>
            <el-select v-model="status" clearable :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in statuss" :key="'status_' + item.id" :label="item.name" :value="item.id"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('temp.time') }}：</label>
            <el-input v-model="search" clearable :placeholder="t('alarm.pleaseInput')" style="flex: 1;" @clear="handleKeydown" @keyup.enter="handleKeydown"></el-input>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="5">
        <el-col :span="12">
          <div class="item">
            <label>{{ t('common.breakTime') }}：</label>
            <el-date-picker v-model="time" :clearable="false" :disabled-date="pickerOptions" :end-placeholder="t('common.endDate')" range-separator="~" :start-placeholder="t('common.startDate')"
            type="datetimerange" @calendar-change="handleChangeStart" @change="handleChange"></el-date-picker>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('temp.label') }}：</label>
            <el-select v-model="selectlabel" clearable collapse-tags multiple :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in labels" :key="item.id" :label="item.name" :value="item.id"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('temp.major') }}：</label>
            <el-select v-model="major" clearable collapse-tags multiple :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in majors" :key="'major_' + item.id" :label="item.name" :value="item.id"></el-option>
            </el-select>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="5">
        <el-col :span="6">
          <div class="item">
            <label>{{ t('temp.eventLevel') }}：</label>
            <el-select v-model="level" clearable collapse-tags multiple :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in levels" :key="'level_' + item.id" :label="item.name" :value="item.id"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">
            <label>{{ t('alarm.mode') }}：</label>
            <el-select v-model="event_type_ids" clearable :placeholder="t('common.pleaseSelect')" @change="handleChange">
              <el-option v-for="item in eventTypes" :key="'eventTypes_' + item.id" :label="item.name" :value="item.id"></el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :offset="9" :span="3">
          <gds-button type="normal-bg" @click="resetFilter">{{ t('common.reset') }}</gds-button>
        </el-col>
      </el-row>
    </section>
    </el-collapse-transition>

    <!-- 条件 -->
    <div class="filter-condition">
      <span class="collapse-icon" :class="showFilter ? '' : 'is-closed'" @click="showFilter = !showFilter">
        <el-icon><DArrowLeft /></el-icon>
      </span>
      <span class="filter-condition-title">
        <img alt="图标" class="condition-icon" src="@/assets/icons/condition.svg" />
        {{t('common.condition')}}：
      </span>
      <span :key="'dc_id_'" class="filter-item">
        <span>【{{t('common.breakTime')}}】{{ dayjs(time[0]).format('YYYY-MM-DD HH:mm:ss') }} - {{dayjs(time[1]).format('YYYY-MM-DD HH:mm:ss')}}</span>
      </span>
      <span v-for="(item, index) in dc_ids" :key="'dc_id_'+ index" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('dc_ids', index)">
        <span>【{{t('alarm.dataCenter')}}】{{ dcs.find(k => k.dc_id === item)?.dc_name }}</span>
      </span>
      <span v-for="(item, index) in level" :key="'rooms_' + index" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('level', index)">
        <span>【{{t('alarm.eventLevel')}}】{{ levels.find(k => k.id === item)?.name  }}</span>
      </span>
      <span v-for="(item, index) in major" :key="'customer_' + index" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('major', index)">
        <span>【{{t('temp.major')}}】{{ majors.find(k => k.id === item)?.name  }}</span>
      </span>
      <span v-if="status || status === 0" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('status')">
        <span>【{{t('common.state')}}】{{ statuss.find(k => k.id === status)?.name }}</span>
      </span>
      <span v-if="event_type_ids" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('event_type_ids')">
        <span>【{{t('alarm.mode')}}】{{ eventTypes.find(k => k.id === event_type_ids)?.name }}</span>
      </span>
      <span v-for="(item, index) in classification" :key="'classification_' + index" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('classification', index)">
        <span>【{{t('alarm.classification')}}】{{ classifications.find(k => k.code === item)?.name  }}</span>
      </span>
      <span v-for="(item, index) in selectlabel" :key="'label_' + index" class="filter-item" @click="deleteCondition('selectlabel', index)">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg"/>
        <span>【{{ t('temp.label') }}】{{  labels.find(k => k.id === item)?.name  }}</span>
      </span>
      <span v-if="keyword" class="filter-item">
        <img alt="图标" class="filter-item-icon" src="@/assets/icons/close.svg" @click="deleteCondition('keyword')">
        <span>【{{t('common.keyword')}}】{{ keyword }}</span>
      </span>
    </div>
    <!-- 列表 -->
    <section class="list">
      <div class="table-section">
        <el-row :gutter="30" >
          <el-col :span="8">
            <gds-button :loading="downLoadLoading" type="primary-bg" @click="exportList">{{t('common.export')}}</gds-button>
          </el-col>
          <el-col class="field" :offset="12" :span="4">
            <FieldFilter :cancelBtnText="t('common.cancel')" :checked="checkedColumns" class="field-filter" :columns="columns"
                          :offset="offsetFeild" :saveBtnText="t('common.save')" :searchText="t('common.search')"
                          :selectAllText="t('common.selectAll')" :showArrow="false" :titleText="t('common.showFiled')"
                          trigger="click" width="180px" @change="handleFieldsChange" @click="handleClickField">
            </FieldFilter>
          </el-col>
        </el-row>

        <div class="table-list">
          <el-table border cell-class-name="custom-table-cell" class="custom-table" :data="tableData" header-cell-class-name="custom-table-header-cell"
            height="100%" :row-class-name="tableRowClassName" @row-click="handleClick" @sort-change="handleSort">
            <el-table-column align="center" label="ID" prop="id" width="80" />
            <el-table-column v-if="checkedColumns.includes(t('alarm.dataCenter'))" align="center" :label="t('alarm.dataCenter')" min-width="120" prop="dc_name" />
            <el-table-column v-if="checkedColumns.includes(t('common.breakTime'))" align="center" :label="t('common.breakTime')" min-width="180" prop="break_time" show-overflow-tooltip sortable="custom">
              <template #default="scope">
                {{ dayjs(scope.row.break_time).format('YYYY-MM-DD HH:mm:ss') }}
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('common.content'))" align="center" :label="t('common.content')" min-width="200" prop="alarm_content" show-overflow-tooltip>
              <template #default="scope">
                {{ scope.row.alarm_content }} <span v-if="scope.row.break_value">({{ scope.row.break_value }})</span>
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('common.state'))" align="center" :label="t('common.state')" min-width="80" prop="status">
              <template #default="scope">
                <span v-if="scope.row.recover_flag === 1">{{ t('alarm.recovered') }}</span>
                <span v-else-if="scope.row.recover_flag === 0">{{ t('alarm.notRecovered') }}</span>
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('common.updateTime'))" align="center" :label="t('common.updateTime')" min-width="180" prop="update_time" show-overflow-tooltip sortable="custom">
              <template #default="scope">
                {{ scope.row.update_time ? dayjs(scope.row.update_time).format('YYYY-MM-DD HH:mm:ss') : '' }}
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('alarm.causeClassification'))" align="center" :label="t('alarm.causeClassification')" min-width="120" prop="reason_type_name" show-overflow-tooltip>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('alarm.detailReason'))" align="center" :label="t('alarm.detailReason')" min-width="120" prop="reason_descr" show-overflow-tooltip>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('alarm.classification'))" align="center" :label="t('alarm.classification')" min-width="80">
              <template #default="scope">
                {{ scope.row.simulate_flag ? t('alarm.mock') : (scope.row.plan_flag === 1 ? t('alarm.inPlan') : t('alarm.unPlan')) }}
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('temp.label'))" align="center" :label="t('temp.label')" min-width="120" prop="label" show-overflow-tooltip>
              <template #default="scope">
                <el-tag v-for="(item, index) in scope.row.event_label" :key="index" style="margin-right: 3px;">{{ item.event_label_name }}</el-tag>
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('temp.eventLevel'))" align="center" :label="t('temp.eventLevel')" min-width="130">
              <template #default="scope">
                <span v-if="scope.row.event_level_id === 1" class="event-level level1"></span>
                <span v-if="scope.row.event_level_id === 2" class="event-level level2"></span>
                <span v-if="scope.row.event_level_id === 3" class="event-level level3"></span>
                <span v-if="scope.row.event_level_id === 4" class="event-level level4"></span> {{ scope.row.event_level_name }}
              </template>
            </el-table-column>
            <el-table-column v-if="checkedColumns.includes(t('temp.major'))" align="center" :label="t('temp.major')" min-width="80"  prop="major_name"/>
            <el-table-column v-if="checkedColumns.includes(t('alarm.mode'))" align="center" :label="t('alarm.mode')" min-width="80"  prop="event_type_name"/>
          </el-table>
        </div>

        <div class="pager">
          <el-pagination v-model:current-page="currentPage" v-model:page-size="pageSize" background
            layout="total, sizes, prev, pager, next, jumper" :page-sizes="[20,50,100,200]" :total="totalSize"
            @current-change="handleCurrentChange" @size-change="handleSizeChange">
          </el-pagination>
        </div>
      </div>
    </section>
    <Logs :alarm="alarm" :open="showLogs" @collapse="showLogs = false"></Logs>
  </div>
  <Contrast class="contrast-alarm" :dcs="dcs" resource="alarm"></Contrast>

</template>

<script setup lang="ts">
import { ref, onMounted, nextTick, computed } from 'vue'
import { getMajors, getAlarms, eventLevels, exportAlarms, eventType, eventLabel } from '@/api/event'
import { getJpiErrorMsg, formatDate, downloadFile, handleBlobResponseError } from '@/util/utils'
import { ElMessage, dayjs } from 'element-plus'
import { useI18n } from 'vue-i18n/dist/vue-i18n.cjs.js'
import { getFuncDcs } from '@/api/dcim'
import Contrast from '@/pages/alarm/views/Contrast.vue'
import Logs from './Logs.vue'

const { t } = useI18n()
const selectlabel = ref([])
const { data } = await getFuncDcs('dcim.menu.alarm.list')
const dcs = ref(data)
// filter相关
const showFilter = ref(true)
const dc_ids = ref([])
const status = ref(null)
const statuss = ref([
  {
    id: 1,
    name: t('alarm.recovered')
  },
  {
    id: 0,
    name: t('alarm.notRecovered')
  }
])

const classifications = ref([
  {
    code: 'simulated',
    name: t('alarm.mock')
  },
  {
    code: 'planned',
    name: t('alarm.inPlan')
  },
  {
    code: 'unplanned',
    name: t('alarm.unPlan')
  }
])

const levels = ref([])
const major = ref([])
const majors = ref([])
const eventTypes = ref([])
const event_type_ids = ref(null)
const level = ref([])
const classification = ref([])
const time = ref<[Date, Date]>([new Date(new Date().toLocaleDateString()), new Date()])
const originTime = ref<[Date, Date]>([new Date(new Date().toLocaleDateString()), new Date()])
const keyword = ref('')
const search = ref('')
const loading = ref(false)

const startTime = ref(null)

const pickerOptions = (time: Date) => {
  if (startTime.value) {
    var date = new Date(startTime.value)
    var date1 = new Date(startTime.value)
    return time.getTime() > new Date(date.setDate(date.getDate() + 31)).valueOf() ||
    time.getTime() > Date.now() || time.getTime() < new Date(date1.setDate(date1.getDate() - 31)).valueOf()
  } else {
    return time.getTime() > Date.now()
  }
}

const handleChangeStart = (val) => {
  startTime.value = val[0]
}
const handleChange = () => {
  initList()
}
const handleKeydown = () => {
  keyword.value = search.value
  initList()
}
// 排序逻辑
const order_by = ref('update_time')
const order = ref<'ASC' | 'DESC' | null>('DESC')
const handleSort = (column: { prop: string; order: string }) => {
  order_by.value = column.prop
  if (column.prop === 'dc_name') {
    order_by.value = 'dc_order'
  }

  order.value = column.order === 'descending' ? 'DESC' : 'ASC'
  initList()
}

const downLoadLoading = ref(false)
const exportList = () => {
  const params = getParams()
  if (totalSize.value > 20000) {
    ElMessage.warning(t('alarm.maxExport'))
    return
  }
  params.zone = Intl.DateTimeFormat().resolvedOptions().timeZone
  downLoadLoading.value = true
  exportAlarms(params).then(res => {
    downloadFile(res)
    downLoadLoading.value = false
  }).catch(err => {
    handleBlobResponseError(err.response, (e) => {
      err.response.data = JSON.parse(e)
      ElMessage.error(err.response.data.error || t('common.requertError'))
    })
    downLoadLoading.value = false
  })
}
// list相关
const tableData = ref([])
const currentPage = ref(1) // 当前页
const totalSize = ref(10) // 总数
const pageSize = ref(50) // 每页数量

const getParams = () => {
  const param: any = {
    limit: pageSize.value,
    offset: (currentPage.value - 1) * pageSize.value,
    end_time: formatDate(dayjs(time.value[1]).format('YYYY-MM-DD HH:mm:ss')),
    start_time: formatDate(dayjs(time.value[0]).format('YYYY-MM-DD HH:mm:ss')),
    dc_ids: dc_ids.value.join(','),
    recover_flag: status.value,
    event_label_ids: selectlabel.value.join(','),
    major_ids: major.value.join(','),
    event_type_ids: event_type_ids.value,
    classification_codes: classification.value.join(','),
    event_level_ids: level.value.join(','),
    alarm_content: keyword.value
  }
  if (order_by.value) {
    param.order_by = order_by.value + ':' + order.value
  }
  return param
}
// 删除过滤条件
const deleteCondition = (type: string, index?: number) => {
  switch (type) {
    case 'major':
      major.value.splice(index, 1)
      break
    case 'selectlabel':
      selectlabel.value.splice(index, 1)
      break
    case 'event_type_ids':
      event_type_ids.value = null
      break
    case 'dc_ids':
      dc_ids.value.splice(index, 1)
      break
    case 'level':
      level.value.splice(index, 1)
      break
    case 'classification':
      classification.value.splice(index, 1)
      break
    case 'status':
      status.value = null
      break
    case 'keyword':
      keyword.value = ''
      search.value = ''
      break
  }

  initList()
}

const initList = async() => {
  loading.value = true
  try {
    const params = getParams()
    const { data } = await getAlarms(params)
    tableData.value = data.data
    totalSize.value = data.total
  } catch (error) {
    console.log(error)
    const msg = getJpiErrorMsg(error)
    ElMessage.error(msg[0] || t(`error.${error.response.data.code}`, error.response.data.data) || t('common.requertError'))
  } finally {
    loading.value = false
  }
}

const labels = ref([])
onMounted(() => {
  initShowFields()
  eventLabel().then(res => {
    labels.value = res.data
  })
  eventLevels().then(res => {
    levels.value = res.data
  })
  getMajors().then(res => {
    majors.value = res.data
  })
  eventType().then(res => {
    eventTypes.value = res.data
  })
  initList()
})
const handleCurrentChange = () => {
  initList()
}
const handleSizeChange = () => {
  initList()
}

const resetFilter = () => {
  dc_ids.value = []
  status.value = null
  level.value = []
  major.value = []
  event_type_ids.value = null
  keyword.value = ''
  classification.value = []
  time.value = originTime.value
  selectlabel.value = []
  initList()
}

const showLogs = ref(false)
const alarm = ref<any>({})
const handleClick = (row) => {
  alarm.value = row
  showLogs.value = true
}
const tableRowClassName = ({
  row
}) => {
  if (row.id === alarm.value.id) {
    return 'select'
  } else {
    return ''
  }
}
// field相关
const COLUMNS_KEY = 'dcim.event.alarm.list.columns'
// 初始化显示字段，从cookie从读取
const initShowFields = () => {
  const value = localStorage.getItem(COLUMNS_KEY)
  if (value) {
    try {
      const columns = JSON.parse(value)
      if (Array.isArray(columns)) {
        checkedColumns.value = columns
      } else {
        throw new TypeError('columns should be array.')
      }
    } catch (e) {
      localStorage.removeItem(COLUMNS_KEY)
    }
  }
}
const handleClickField = () => {
  viewport()
}
// 计算视口
const offsetFeild = ref(0)
const viewport = () => {
  nextTick(() => {
    const dom: HTMLImageElement = document.querySelector('.field-filter')
    const offsetTop = dom.offsetTop + 40
    const clientHeight = document.body.clientHeight
    const feildHeight = 550
    if ((clientHeight - offsetTop) < feildHeight) {
      offsetFeild.value = -(feildHeight - (clientHeight - offsetTop))
    }
  })
}
const columns = computed(() => {
  return [
    { label: t('common.ID'), disabled: true},
    { label: t('alarm.dataCenter'), disabled: false},
    { label: t('common.breakTime'), disabled: false},
    { label: t('common.content'), disabled: false},
    { label: t('common.state'), disabled: false},
    { label: t('common.updateTime'), disabled: false},
    { label: t('alarm.causeClassification'), disabled: false},
    { label: t('alarm.detailReason'), disabled: false},
    { label: t('alarm.classification'), disabled: false},
    { label: t('temp.label'), disabled: false},
    { label: t('temp.eventLevel'), disabled: false},
    { label: t('temp.major'), disabled: false},
    { label: t('alarm.mode'), disabled: false}
  ]
})
const checkedColumns = ref([t('common.ID'), t('alarm.dataCenter'), t('common.breakTime'), t('common.content'), t('common.state'), t('common.updateTime'),
  t('alarm.causeClassification'), t('alarm.classification'), t('temp.label'), t('temp.eventLevel')])
const handleFieldsChange = (value: string[]) => {
  checkedColumns.value = value
  localStorage.setItem(COLUMNS_KEY, JSON.stringify(value))
}
</script>

<style lang="stylus" scoped>
@import '../../styles/common.styl';
:deep(.el-table .select td)
  border-top 1px solid #43AFFD
  border-bottom 1px solid #43AFFD

  &:last-child
    border-right 1px solid #43AFFD
  &:first-child
    border-left 1px solid #43AFFD

.temp
  height 100%
  width 100%
  display flex
  -webkit-box-orient vertical
  -webkit-box-direction normal
  -ms-flex-direction column
  flex-direction column
  position relative
  overflow hidden

  .filter
    padding 20px 20px 10px
    background #fff

    .item
      display flex
      align-items center

      label
        width 100px
        text-align right
        display inline-block

      :deep(.el-select)
        flex 1

    :deep(.el-row)
      margin-bottom 16px

    label
      font-size 14px
      color #595959

  .filter-condition
    position relative
    padding 20px 15px
    font-size 14px
    color rgba(89, 89, 89, 1)

    &-title
      display inline-flex
      align-items center

      .condition-icon
        margin-right 5px
        height 12px
    .collapse-icon
      cursor pointer
      position absolute
      top -18px
      right 10px
      display inline-block
      width 36px
      height 36px
      display flex
      align-items center
      justify-content center
      background: #f0f3f6;
      box-shadow: 0 3px 8px 3px #bbb;
      border-radius: 50%;
      transition top 0.3s

      .el-icon
        font-size 18px
        color #959595
        transition transform 0.3s
        transform rotate(90deg)

      &.is-closed
        top -10px
        .el-icon
          transform rotate(270deg)
    .filter-item
      display inline-block
      position relative
      padding-right 10px
      margin-right 10px
      color rgba(81, 181, 254, 1)
      font-size 14px
      word-break keep-all

      &-icon
        cursor pointer
        position absolute
        width 10px
        top 0
        right 0

  .list
    flex 1
    background #fff
    position relative

    .table-section
      padding 20px
      height calc(100% - 50px)
      overflow hidden
      position absolute
      width 100%
      box-sizing border-box

      .field
        display flex
        align-items flex-end
        justify-content flex-end

    .table-list
      height calc(100% - 100px)
      margin-top 16px

      .baifenbi
        width 65px
        min-height 1px
        display inline-block

      .status
        font-size 14px
        color #515151
        border 1px solid rgba(222, 222, 222, 1)
        padding 4px 30px
        display inline-block
        border-radius 20px

        &.status_2
          color rgba(76, 179, 253, 1)
          border-color rgba(76, 179, 253, 1)

        &.status_3, &.status_4
          color #0b9c0b
          border-color #0b9c0b
</style>
```

#### Output:

```json
{
  "translatedCode": "<template>\n  <div v-loading=\"loading\" class=\"temp\">\n    <!-- Condition Selection -->\n    <el-collapse-transition>\n      <section v-show=\"showFilter\" class=\"filter\" >\n      <el-row :gutter=\"5\">\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('alarm.dataCenter') }}：</label>\n            <el-select v-model=\"dc_ids\" clearable collapse-tags collapse-tags-tooltip filterable multiple\n              :placeholder=\"t('common.pleaseSelect')\"\n              @change=\"handleChange\">\n              <el-option v-for=\"item in dcs\" :key=\"item.dc_id\" :label=\"item.dc_name\" :value=\"item.dc_id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('alarm.classification') }}：</label>\n            <el-select v-model=\"classification\" clearable collapse-tags multiple :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in classifications\" :key=\"'status_' + item.code\" :label=\"item.name\" :value=\"item.code\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('common.state') }}：</label>\n            <el-select v-model=\"status\" clearable :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in statuss\" :key=\"'status_' + item.id\" :label=\"item.name\" :value=\"item.id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('temp.time') }}：</label>\n            <el-input v-model=\"search\" clearable :placeholder=\"t('alarm.pleaseInput')\" style=\"flex: 1;\" @clear=\"handleKeydown\" @keyup.enter=\"handleKeydown\"></el-input>\n          </div>\n        </el-col>\n      </el-row>\n      <el-row :gutter=\"5\">\n        <el-col :span=\"12\">\n          <div class=\"item\">\n            <label>{{ t('common.breakTime') }}：</label>\n            <el-date-picker v-model=\"time\" :clearable=\"false\" :disabled-date=\"pickerOptions\" :end-placeholder=\"t('common.endDate')\" range-separator=\"~\" :start-placeholder=\"t('common.startDate')\"\n            type=\"datetimerange\" @calendar-change=\"handleChangeStart\" @change=\"handleChange\"></el-date-picker>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('temp.label') }}：</label>\n            <el-select v-model=\"selectlabel\" clearable collapse-tags multiple :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in labels\" :key=\"item.id\" :label=\"item.name\" :value=\"item.id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('temp.major') }}：</label>\n            <el-select v-model=\"major\" clearable collapse-tags multiple :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in majors\" :key=\"'major_' + item.id\" :label=\"item.name\" :value=\"item.id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n      </el-row>\n      <el-row :gutter=\"5\">\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('temp.eventLevel') }}：</label>\n            <el-select v-model=\"level\" clearable collapse-tags multiple :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in levels\" :key=\"'level_' + item.id\" :label=\"item.name\" :value=\"item.id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :span=\"6\">\n          <div class=\"item\">\n            <label>{{ t('alarm.mode') }}：</label>\n            <el-select v-model=\"event_type_ids\" clearable :placeholder=\"t('common.pleaseSelect')\" @change=\"handleChange\">\n              <el-option v-for=\"item in eventTypes\" :key=\"'eventTypes_' + item.id\" :label=\"item.name\" :value=\"item.id\"></el-option>\n            </el-select>\n          </div>\n        </el-col>\n        <el-col :offset=\"9\" :span=\"3\">\n          <gds-button type=\"normal-bg\" @click=\"resetFilter\">{{ t('common.reset') }}</gds-button>\n        </el-col>\n      </el-row>\n    </section>\n    </el-collapse-transition>\n\n    <!-- Conditions -->\n    <div class=\"filter-condition\">\n      <span class=\"collapse-icon\" :class=\"showFilter ? '' : 'is-closed'\" @click=\"showFilter = !showFilter\">\n        <el-icon><DArrowLeft /></el-icon>\n      </span>\n      <span class=\"filter-condition-title\">\n        <img alt=\"Icon\" class=\"condition-icon\" src=\"@/assets/icons/condition.svg\" />\n        {{t('common.condition')}}：\n      </span>\n      <span :key=\"'dc_id_'\" class=\"filter-item\">\n        <span>【{{t('common.breakTime')}}】{{ dayjs(time[0]).format('YYYY-MM-DD HH:mm:ss') }} - {{dayjs(time[1]).format('YYYY-MM-DD HH:mm:ss')}}</span>\n      </span>\n      <span v-for=\"(item, index) in dc_ids\" :key=\"'dc_id_'+ index\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('dc_ids', index)\">\n        <span>【{{t('alarm.dataCenter')}}】{{ dcs.find(k => k.dc_id === item)?.dc_name }}</span>\n      </span>\n      <span v-for=\"(item, index) in level\" :key=\"'rooms_' + index\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('level', index)\">\n        <span>【{{t('alarm.eventLevel')}}】{{ levels.find(k => k.id === item)?.name  }}</span>\n      </span>\n      <span v-for=\"(item, index) in major\" :key=\"'customer_' + index\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('major', index)\">\n        <span>【{{t('temp.major')}}】{{ majors.find(k => k.id === item)?.name  }}</span>\n      </span>\n      <span v-if=\"status || status === 0\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('status')\">\n        <span>【{{t('common.state')}}】{{ statuss.find(k => k.id === status)?.name }}</span>\n      </span>\n      <span v-if=\"event_type_ids\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('event_type_ids')\">\n        <span>【{{t('alarm.mode')}}】{{ eventTypes.find(k => k.id === event_type_ids)?.name }}</span>\n      </span>\n      <span v-for=\"(item, index) in classification\" :key=\"'classification_' + index\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('classification', index)\">\n        <span>【{{t('alarm.classification')}}】{{ classifications.find(k => k.code === item)?.name  }}</span>\n      </span>\n      <span v-for=\"(item, index) in selectlabel\" :key=\"'label_' + index\" class=\"filter-item\" @click=\"deleteCondition('selectlabel', index)\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\"/>\n        <span>【{{ t('temp.label') }}】{{  labels.find(k => k.id === item)?.name  }}</span>\n      </span>\n      <span v-if=\"keyword\" class=\"filter-item\">\n        <img alt=\"Icon\" class=\"filter-item-icon\" src=\"@/assets/icons/close.svg\" @click=\"deleteCondition('keyword')\">\n        <span>【{{t('common.keyword')}}】{{ keyword }}</span>\n      </span>\n    </div>\n    <!-- List -->\n    <section class=\"list\">\n      <div class=\"table-section\">\n        <el-row :gutter=\"30\" >\n          <el-col :span=\"8\">\n            <gds-button :loading=\"downLoadLoading\" type=\"primary-bg\" @click=\"exportList\">{{t('common.export')}}</gds-button>\n          </el-col>\n          <el-col class=\"field\" :offset=\"12\" :span=\"4\">\n            <FieldFilter :cancelBtnText=\"t('common.cancel')\" :checked=\"checkedColumns\" class=\"field-filter\" :columns=\"columns\"\n                          :offset=\"offsetFeild\" :saveBtnText=\"t('common.save')\" :searchText=\"t('common.search')\"\n                          :selectAllText=\"t('common.selectAll')\" :showArrow=\"false\" :titleText=\"t('common.showFiled')\"\n                          trigger=\"click\" width=\"180px\" @change=\"handleFieldsChange\" @click=\"handleClickField\">\n            </FieldFilter>\n          </el-col>\n        </el-row>\n\n        <div class=\"table-list\">\n          <el-table border cell-class-name=\"custom-table-cell\" class=\"custom-table\" :data=\"tableData\" header-cell-class-name=\"custom-table-header-cell\"\n            height=\"100%\" :row-class-name=\"tableRowClassName\" @row-click=\"handleClick\" @sort-change=\"handleSort\">\n            <el-table-column align=\"center\" label=\"ID\" prop=\"id\" width=\"80\" />\n            <el-table-column v-if=\"checkedColumns.includes(t('alarm.dataCenter'))\" align=\"center\" :label=\"t('alarm.dataCenter')\" min-width=\"120\" prop=\"dc_name\" />\n            <el-table-column v-if=\"checkedColumns.includes(t('common.breakTime'))\" align=\"center\" :label=\"t('common.breakTime')\" min-width=\"180\" prop=\"break_time\" show-overflow-tooltip sortable=\"custom\">\n              <template #default=\"scope\">\n                {{ dayjs(scope.row.break_time).format('YYYY-MM-DD HH:mm:ss') }}\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('common.content'))\" align=\"center\" :label=\"t('common.content')\" min-width=\"200\" prop=\"alarm_content\" show-overflow-tooltip>\n              <template #default=\"scope\">\n                {{ scope.row.alarm_content }} <span v-if=\"scope.row.break_value\">({{ scope.row.break_value }})</span>\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('common.state'))\" align=\"center\" :label=\"t('common.state')\" min-width=\"80\" prop=\"status\">\n              <template #default=\"scope\">\n                <span v-if=\"scope.row.recover_flag === 1\">{{ t('alarm.recovered') }}</span>\n                <span v-else-if=\"scope.row.recover_flag === 0\">{{ t('alarm.notRecovered') }}</span>\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('common.updateTime'))\" align=\"center\" :label=\"t('common.updateTime')\" min-width=\"180\" prop=\"update_time\" show-overflow-tooltip sortable=\"custom\">\n              <template #default=\"scope\">\n                {{ scope.row.update_time ? dayjs(scope.row.update_time).format('YYYY-MM-DD HH:mm:ss') : '' }}\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('alarm.causeClassification'))\" align=\"center\" :label=\"t('alarm.causeClassification')\" min-width=\"120\" prop=\"reason_type_name\" show-overflow-tooltip>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('alarm.detailReason'))\" align=\"center\" :label=\"t('alarm.detailReason')\" min-width=\"120\" prop=\"reason_descr\" show-overflow-tooltip>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('alarm.classification'))\" align=\"center\" :label=\"t('alarm.classification')\" min-width=\"80\">\n              <template #default=\"scope\">\n                {{ scope.row.simulate_flag ? t('alarm.mock') : (scope.row.plan_flag === 1 ? t('alarm.inPlan') : t('alarm.unPlan')) }}\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('temp.label'))\" align=\"center\" :label=\"t('temp.label')\" min-width=\"120\" prop=\"label\" show-overflow-tooltip>\n              <template #default=\"scope\">\n                <el-tag v-for=\"(item, index) in scope.row.event_label\" :key=\"index\" style=\"margin-right: 3px;\">{{ item.event_label_name }}</el-tag>\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('temp.eventLevel'))\" align=\"center\" :label=\"t('temp.eventLevel')\" min-width=\"130\">\n              <template #default=\"scope\">\n                <span v-if=\"scope.row.event_level_id === 1\" class=\"event-level level1\"></span>\n                <span v-if=\"scope.row.event_level_id === 2\" class=\"event-level level2\"></span>\n                <span v-if=\"scope.row.event_level_id === 3\" class=\"event-level level3\"></span>\n                <span v-if=\"scope.row.event_level_id === 4\" class=\"event-level level4\"></span> {{ scope.row.event_level_name }}\n              </template>\n            </el-table-column>\n            <el-table-column v-if=\"checkedColumns.includes(t('temp.major'))\" align=\"center\" :label=\"t('temp.major')\" min-width=\"80\"  prop=\"major_name\"/>\n            <el-table-column v-if=\"checkedColumns.includes(t('alarm.mode'))\" align=\"center\" :label=\"t('alarm.mode')\" min-width=\"80\"  prop=\"event_type_name\"/>\n          </el-table>\n        </div>\n\n        <div class=\"pager\">\n          <el-pagination v-model:current-page=\"currentPage\" v-model:page-size=\"pageSize\" background\n            layout=\"total, sizes, prev, pager, next, jumper\" :page-sizes=\"[20,50,100,200]\" :total=\"totalSize\"\n            @current-change=\"handleCurrentChange\" @size-change=\"handleSizeChange\">\n          </el-pagination>\n        </div>\n      </div>\n    </section>\n    <Logs :alarm=\"alarm\" :open=\"showLogs\" @collapse=\"showLogs = false\"></Logs>\n  </div>\n  <Contrast class=\"contrast-alarm\" :dcs=\"dcs\" resource=\"alarm\"></Contrast>\n\n</template>\n\n<script setup lang=\"ts\">\nimport { ref, onMounted, nextTick, computed } from 'vue'\nimport { getMajors, getAlarms, eventLevels, exportAlarms, eventType, eventLabel } from '@/api/event'\nimport { getJpiErrorMsg, formatDate, downloadFile, handleBlobResponseError } from '@/util/utils'\nimport { ElMessage, dayjs } from 'element-plus'\nimport { useI18n } from 'vue-i18n/dist/vue-i18n.cjs.js'\nimport { getFuncDcs } from '@/api/dcim'\nimport Contrast from '@/pages/alarm/views/Contrast.vue'\nimport Logs from './Logs.vue'\n\nconst { t } = useI18n()\nconst selectlabel = ref([])\nconst { data } = await getFuncDcs('dcim.menu.alarm.list')\nconst dcs = ref(data)\n// filter related\nconst showFilter = ref(true)\nconst dc_ids = ref([])\nconst status = ref(null)\nconst statuss = ref([\n  {\n    id: 1,\n    name: t('alarm.recovered')\n  },\n  {\n    id: 0,\n    name: t('alarm.notRecovered')\n  }\n])\n\nconst classifications = ref([\n  {\n    code: 'simulated',\n    name: t('alarm.mock')\n  },\n  {\n    code: 'planned',\n    name: t('alarm.inPlan')\n  },\n  {\n    code: 'unplanned',\n    name: t('alarm.unPlan')\n  }\n])\n\nconst levels = ref([])\nconst major = ref([])\nconst majors = ref([])\nconst eventTypes = ref([])\nconst event_type_ids = ref(null)\nconst level = ref([])\nconst classification = ref([])\nconst time = ref<[Date, Date]>([new Date(new Date().toLocaleDateString()), new Date()])\nconst originTime = ref<[Date, Date]>([new Date(new Date().toLocaleDateString()), new Date()])\nconst keyword = ref('')\nconst search = ref('')\nconst loading = ref(false)\n\nconst startTime = ref(null)\n\nconst pickerOptions = (time: Date) => {\n  if (startTime.value) {\n    var date = new Date(startTime.value)\n    var date1 = new Date(startTime.value)\n    return time.getTime() > new Date(date.setDate(date.getDate() + 31)).valueOf() ||\n    time.getTime() > Date.now() || time.getTime() < new Date(date1.setDate(date1.getDate() - 31)).valueOf()\n  } else {\n    return time.getTime() > Date.now()\n  }\n}\n\nconst handleChangeStart = (val) => {\n  startTime.value = val[0]\n}\nconst handleChange = () => {\n  initList()\n}\nconst handleKeydown = () => {\n  keyword.value = search.value\n  initList()\n}\n// Sorting logic\nconst order_by = ref('update_time')\nconst order = ref<'ASC' | 'DESC' | null>('DESC')\nconst handleSort = (column: { prop: string; order: string }) => {\n  order_by.value = column.prop\n  if (column.prop === 'dc_name') {\n    order_by.value = 'dc_order'\n  }\n\n  order.value = column.order === 'descending' ? 'DESC' : 'ASC'\n  initList()\n}\n\nconst downLoadLoading = ref(false)\nconst exportList = () => {\n  const params = getParams()\n  if (totalSize.value > 20000) {\n    ElMessage.warning(t('alarm.maxExport'))\n    return\n  }\n  params.zone = Intl.DateTimeFormat().resolvedOptions().timeZone\n  downLoadLoading.value = true\n  exportAlarms(params).then(res => {\n    downloadFile(res)\n    downLoadLoading.value = false\n  }).catch(err => {\n    handleBlobResponseError(err.response, (e) => {\n      err.response.data = JSON.parse(e)\n      ElMessage.error(err.response.data.error || t('common.requertError'))\n    })\n    downLoadLoading.value = false\n  })\n}\n// list related\nconst tableData = ref([])\nconst currentPage = ref(1) // current page\nconst totalSize = ref(10) // total count\nconst pageSize = ref(50) // items per page\n\nconst getParams = () => {\n  const param: any = {\n    limit: pageSize.value,\n    offset: (currentPage.value - 1) * pageSize.value,\n    end_time: formatDate(dayjs(time.value[1]).format('YYYY-MM-DD HH:mm:ss')),\n    start_time: formatDate(dayjs(time.value[0]).format('YYYY-MM-DD HH:mm:ss')),\n    dc_ids: dc_ids.value.join(','),\n    recover_flag: status.value,\n    event_label_ids: selectlabel.value.join(','),\n    major_ids: major.value.join(','),\n    event_type_ids: event_type_ids.value,\n    classification_codes: classification.value.join(','),\n    event_level_ids: level.value.join(','),\n    alarm_content: keyword.value\n  }\n  if (order_by.value) {\n    param.order_by = order_by.value + ':' + order.value\n  }\n  return param\n}\n// Delete filter conditions\nconst deleteCondition = (type: string, index?: number) => {\n  switch (type) {\n    case 'major':\n      major.value.splice(index, 1)\n      break\n    case 'selectlabel':\n      selectlabel.value.splice(index, 1)\n      break\n    case 'event_type_ids':\n      event_type_ids.value = null\n      break\n    case 'dc_ids':\n      dc_ids.value.splice(index, 1)\n      break\n    case 'level':\n      level.value.splice(index, 1)\n      break\n    case 'classification':\n      classification.value.splice(index, 1)\n      break\n    case 'status':\n      status.value = null\n      break\n    case 'keyword':\n      keyword.value = ''\n      search.value = ''\n      break\n  }\n\n  initList()\n}\n\nconst initList = async() => {\n  loading.value = true\n  try {\n    const params = getParams()\n    const { data } = await getAlarms(params)\n    tableData.value = data.data\n    totalSize.value = data.total\n  } catch (error) {\n    console.log(error)\n    const msg = getJpiErrorMsg(error)\n    ElMessage.error(msg[0] || t(`error.${error.response.data.code}`, error.response.data.data) || t('common.requertError'))\n  } finally {\n    loading.value = false\n  }\n}\n\nconst labels = ref([])\nonMounted(() => {\n  initShowFields()\n  eventLabel().then(res => {\n    labels.value = res.data\n  })\n  eventLevels().then(res => {\n    levels.value = res.data\n  })\n  getMajors().then(res => {\n    majors.value = res.data\n  })\n  eventType().then(res => {\n    eventTypes.value = res.data\n  })\n  initList()\n})\nconst handleCurrentChange = () => {\n  initList()\n}\nconst handleSizeChange = () => {\n  initList()\n}\n\nconst resetFilter = () => {\n  dc_ids.value = []\n  status.value = null\n  level.value = []\n  major.value = []\n  event_type_ids.value = null\n  keyword.value = ''\n  classification.value = []\n  time.value = originTime.value\n  selectlabel.value = []\n  initList()\n}\n\nconst showLogs = ref(false)\nconst alarm = ref<any>({})\nconst handleClick = (row) => {\n  alarm.value = row\n  showLogs.value = true\n}\nconst tableRowClassName = ({\n  row\n}) => {\n  if (row.id === alarm.value.id) {\n    return 'select'\n  } else {\n    return ''\n  }\n}\n// field related\nconst COLUMNS_KEY = 'dcim.event.alarm.list.columns'\n// Initialize displayed fields, read from cookie\nconst initShowFields = () => {\n  const value = localStorage.getItem(COLUMNS_KEY)\n  if (value) {\n    try {\n      const columns = JSON.parse(value)\n      if (Array.isArray(columns)) {\n        checkedColumns.value = columns\n      } else {\n        throw new TypeError('columns should be array.')\n      }\n    } catch (e) {\n      localStorage.removeItem(COLUMNS_KEY)\n    }\n  }\n}\nconst handleClickField = () => {\n  viewport()\n}\n// Calculate viewport\nconst offsetFeild = ref(0)\nconst viewport = () => {\n  nextTick(() => {\n    const dom: HTMLImageElement = document.querySelector('.field-filter')\n    const offsetTop = dom.offsetTop + 40\n    const clientHeight = document.body.clientHeight\n    const feildHeight = 550\n    if ((clientHeight - offsetTop) < feildHeight) {\n      offsetFeild.value = -(feildHeight - (clientHeight - offsetTop))\n    }\n  })\n}\nconst columns = computed(() => {\n  return [\n    { label: t('common.ID'), disabled: true},\n    { label: t('alarm.dataCenter'), disabled: false},\n    { label: t('common.breakTime'), disabled: false},\n    { label: t('common.content'), disabled: false},\n    { label: t('common.state'), disabled: false},\n    { label: t('common.updateTime'), disabled: false},\n    { label: t('alarm.causeClassification'), disabled: false},\n    { label: t('alarm.detailReason'), disabled: false},\n    { label: t('alarm.classification'), disabled: false},\n    { label: t('temp.label'), disabled: false},\n    { label: t('temp.eventLevel'), disabled: false},\n    { label: t('temp.major'), disabled: false},\n    { label: t('alarm.mode'), disabled: false}\n  ]\n})\nconst checkedColumns = ref([t('common.ID'), t('alarm.dataCenter'), t('common.breakTime'), t('common.content'), t('common.state'), t('common.updateTime'),\n  t('alarm.causeClassification'), t('alarm.classification'), t('temp.label'), t('temp.eventLevel')])\nconst handleFieldsChange = (value: string[]) => {\n  checkedColumns.value = value\n  localStorage.setItem(COLUMNS_KEY, JSON.stringify(value))\n}\n</script>\n\n<style lang=\"stylus\" scoped>\n@import '../../styles/common.styl';\n:deep(.el-table .select td)\n  border-top 1px solid #43AFFD\n  border-bottom 1px solid #43AFFD\n\n  &:last-child\n    border-right 1px solid #43AFFD\n  &:first-child\n    border-left 1px solid #43AFFD\n\n.temp\n  height 100%\n  width 100%\n  display flex\n  -webkit-box-orient vertical\n  -webkit-box-direction normal\n  -ms-flex-direction column\n  flex-direction column\n  position relative\n  overflow hidden\n\n  .filter\n    padding 20px 20px 10px\n    background #fff\n\n    .item\n      display flex\n      align-items center\n\n      label\n        width 100px\n        text-align right\n        display inline-block\n\n      :deep(.el-select)\n        flex 1\n\n    :deep(.el-row)\n      margin-bottom 16px\n\n    label\n      font-size 14px\n      color #595959\n\n  .filter-condition\n    position relative\n    padding 20px 15px\n    font-size 14px\n    color rgba(89, 89, 89, 1)\n\n    &-title\n      display inline-flex\n      align-items center\n\n      .condition-icon\n        margin-right 5px\n        height 12px\n    .collapse-icon\n      cursor pointer\n      position absolute\n      top -18px\n      right 10px\n      display inline-block\n      width 36px\n      height 36px\n      display flex\n      align-items center\n      justify-content center\n      background: #f0f3f6;\n      box-shadow: 0 3px 8px 3px #bbb;\n      border-radius: 50%;\n      transition top 0.3s\n\n      .el-icon\n        font-size 18px\n        color #959595\n        transition transform 0.3s\n        transform rotate(90deg)\n\n      &.is-closed\n        top -10px\n        .el-icon\n          transform rotate(270deg)\n    .filter-item\n      display inline-block\n      position relative\n      padding-right 10px\n      margin-right 10px\n      color rgba(81, 181, 254, 1)\n      font-size 14px\n      word-break keep-all\n\n      &-icon\n        cursor pointer\n        position absolute\n        width 10px\n        top 0\n        right 0\n\n  .list\n    flex 1\n    background #fff\n    position relative\n\n    .table-section\n      padding 20px\n      height calc(100% - 50px)\n      overflow hidden\n      position absolute\n      width 100%\n      box-sizing border-box\n\n      .field\n        display flex\n        align-items flex-end\n        justify-content flex-end\n\n    .table-list\n      height calc(100% - 100px)\n      margin-top 16px\n\n      .baifenbi\n        width 65px\n        min-height 1px\n        display inline-block\n\n      .status\n        font-size 14px\n        color #515151\n        border 1px solid rgba(222, 222, 222, 1)\n        padding 4px 30px\n        display inline-block\n        border-radius 20px\n\n        &.status_2\n          color rgba(76, 179, 253, 1)\n          border-color rgba(76, 179, 253, 1)\n\n        &.status_3, &.status_4\n          color #0b9c0b\n          border-color #0b9c0b\n</style>",
  "details": [
    {
      "lineNumber": 3,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "条件选择",
      "translatedText": "Condition Selection"
    },
    {
      "lineNumber": 95,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "条件",
      "translatedText": "Conditions"
    },
    {
      "lineNumber": 101,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 108,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 112,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 116,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 120,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 124,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 128,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 132,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 136,
      "lineType": "Tag Attribute",
      "jobType": "Text Translation",
      "originalText": "图标",
      "translatedText": "Icon"
    },
    {
      "lineNumber": 140,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "列表",
      "translatedText": "List"
    },
    {
      "lineNumber": 300,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "排序逻辑",
      "translatedText": "Sorting logic"
    },
    {
      "lineNumber": 333,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "list相关",
      "translatedText": "list related"
    },
    {
      "lineNumber": 335,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "当前页",
      "translatedText": "Current Page"
    },
    {
      "lineNumber": 336,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "总数",
      "translatedText": "total"
    },
    {
      "lineNumber": 337,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "每页数量",
      "translatedText": "Quantity per page"
    },
    {
      "lineNumber": 359,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "删除过滤条件",
      "translatedText": "Delete filter conditions"
    },
    {
      "lineNumber": 459,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "field相关",
      "translatedText": "field related"
    },
    {
      "lineNumber": 461,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "初始化显示字段，从cookie从读取",
      "translatedText": "Initialize displayed fields, read from cookie"
    },
    {
      "lineNumber": 480,
      "lineType": "Comment",
      "jobType": "Text Translation",
      "originalText": "计算视口",
      "translatedText": "Calculate viewport"
    }
  ]
}
```