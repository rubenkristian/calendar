<template>
  <div class="calendar">
    <div class="calendar__days">
      <div class="days__text"  v-for="day in days">
        {{ day }}
      </div>
    </div>
    <div class="calendar_content">
      <div class="day_content" v-for="date in date_list">
        <Days :dates="date" @on-day-click="onSelect" @on-unselected="onUnselect"/>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { onMounted, reactive, ref, toRefs, watch } from 'vue';
  import Days from './Days.vue';

  const days = ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"];
  let date_list = reactive([]);

  const props = defineProps({
    month: {type: Number, default: 1},
    year: Number,
  })

  const { month, year } = toRefs(props);

  function updateCalendar(select_start, select_end, year, month) {
    date_list.splice(0, date_list.length);
    const today = new Date();
    const first_date_current = new Date(year, month - 1, 1);
    const first_date_next = new Date(year, month, 0);

    const day_first_date_current = first_date_current.getDay() === 0 ? 6 : first_date_current.getDay() - 1;
    const day_first_date_next = first_date_next.getDay() === 0 ? 6 : first_date_next.getDay() - 1;
    const start_date = new Date(year, month - 1, -(day_first_date_current - 1));
    const end_date = new Date(year, month, (6 - day_first_date_next))

    let week_days = [];
    let date = start_date;
    let a = 0;

    while (date.getTime() < end_date.getTime()) {
      date = new Date(year, month - 1, (-(day_first_date_current - 1) + a));
      
      if (week_days.length === 7) {
        date_list.push(week_days);
        week_days = [];
      }
      
      week_days.push({
        index: a,
        date: date,
        thisMonth: date.getMonth() === month - 1,
        isSelected: a >= select_start && a <= select_end,
        text: date.getDate(),
        today: today.getDate() === date.getDate() && today.getMonth() === date.getMonth(),
        previousDay: today.getDate() <= date.getDate() && today.getMonth() <= date.getMonth() && today.getFullYear() <= date.getFullYear(),
      })
      a++;
    }

    date_list.push(week_days)
  }

  onMounted(() => {
    updateCalendar(-1, -1, year.value, month.value)
  })

  watch(month, async (nMonth, oMonth) => {
    updateCalendar(-1, -1, year.value, nMonth)
  })

  function onSelect(date, index) {
    updateCalendar(index, index + 3, year.value, month.value)
  }

  function onUnselect() {
    updateCalendar(-1, -1, year.value, month.value)
  }
</script>

<style>
  .calendar {
    margin: 50px;
    display: flex;
    flex-direction: column;
    width: 676px;
  }

  .calendar__days {
    display: flex;
    align-items: center;
    text-align: center;
    gap: 5px;
    color: #D3C9D1;
    font-weight: 600;
    font-size: 14px;
    line-height: 20px;
  }

  .calendar__days .days__text {
    width: 92px;
    margin: 0 0 16px 0;
  }

  .calendar_content {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
  }

  .calendar_content .day_content {
    width: 100%;
    display: flex;
    gap: 5.33px;
  }
</style>