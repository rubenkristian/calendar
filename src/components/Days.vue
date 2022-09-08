<script setup>
import { ref } from 'vue';

  let selected_index = ref(-1)
  const count_days = 7;

  const props = defineProps({
    dates: Array
  })

  const emits = defineEmits({
    onDayClick: ({date, index}) => {
      return true;
    },
    onUnselected: null,
  })

  function dayClick(active, date, index) {
    if (index === selected_index) {
      active && emits('onUnselected');
      selected_index = -1
    } else {
      active && emits('onDayClick', date, index);
      selected_index = index
    }
  }
</script>

<template>
  <div v-for="day in count_days" :class="{ day: (dates[day - 1] !== undefined && dates[day - 1].thisMonth), none: !(dates[day - 1] !== undefined && dates[day - 1].thisMonth), selected: (dates[day - 1] !== undefined && dates[day - 1].isSelected) }" @click="dayClick(dates[day - 1] !== undefined && dates[day - 1].thisMonth, dates[day - 1].date, dates[day - 1].index)">
    <svg v-if="dates[day - 1] !== undefined && dates[day - 1].today" class="today" width="23" height="23" viewBox="0 0 23 23" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M23 23L0 4.01591e-05H19.5C21.6046 4.01591e-05 23 2.52593 23 3.65281V23Z" fill="#FF60BF"/>
    </svg>
    <span>{{ dates[day - 1].text.toString().trim() }}</span>
  </div>
</template>

<style>
  .day {
    position: relative;
    width: 92px;
    height: 72px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color:#F7F7F7;
    border-radius: 4px;
    color: #363636;
    font-weight: 600;
    font-size: 14px;
    cursor: pointer;
    user-select: none;
  }

  .selected {
    background-color: coral;
    color: #F7F7F7;
  }

  .none {
    position: relative;
    width: 92px;
    height: 72px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    color: #363636;
    font-weight: 600;
    font-size: 14px;
    cursor: not-allowed;
    user-select: none;
  }

  .today {
    position: absolute;
    width: 23px;
    height: 23px;
    top: 0px;
    right: 0;
  }
</style>