<script setup>
  const count_days = 7;

  const props = defineProps({
    dates: Array
  })
</script>

<template>
  <div v-for="day in count_days" :class="(dates[day - 1] !== undefined && dates[day - 1].thisMonth) ? 'day' : 'none'">
    <svg v-if="dates[day - 1] !== undefined && dates[day - 1].today" class="today" width="23" height="23" viewBox="0 0 23 23" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M23 23L0 4.01591e-05H19.5C21.6046 4.01591e-05 23 2.52593 23 3.65281V23Z" fill="#FF60BF"/>
    </svg>

    {{ (dates[day - 1] !== undefined && dates[day - 1].thisMonth) ? dates[day - 1].text : '' }}
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
  }

  .none {
    width: 92px;
    height: 72px;
    display: flex;
  }

  .today {
    position: absolute;
    width: 23px;
    height: 23px;
    top: 0px;
    right: 0;
  }
</style>