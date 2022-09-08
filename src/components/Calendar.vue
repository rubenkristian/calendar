<template>
  <div class="calendar">
    <div class="calendar__days">
      <div class="days__text"  v-for="day in days">
        {{ day }}
      </div>
    </div>
    <div class="calendar_content">
      <div class="day_content" v-for="date in date_list" @click="hello">
        <Days :dates="date"/>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { reactive, ref, toRefs, watch } from 'vue';
  import Days from './Days.vue';

  const days = ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"];
  let date_list = reactive([]);
  let count_week = ref(0);
  let first_monday = ref(0);

  const props = defineProps({
    month: {type: Number, default: 1},
    year: Number,
  })

  const { month, year } = toRefs(props);

  const today = new Date();
  const first_date = new Date(year.value, month.value - 1, 1);
  const last_date = new Date(year.value, month.value, 0);
  count_week = Math.ceil((last_date.getDate() + first_date.getDay() - 1) / 7);
  first_monday = first_date.getDay() - 1

  let week_days = [];
  for (let i = 0 - (first_date.getDay() - 1); i < (last_date.getDate()); i++) {
    if (week_days.length === 7) {
      date_list.push(week_days);
      week_days = [];
    }
    week_days.push({
      thisMonth: i >= 0,
      text: (i + 1),
      today: today.getDate() === (i + 1) && today.getMonth() === (month.value - 1),
      previousDay: today.getDate() <= (i + 1) && today.getMonth() <= (month.value - 1) && today.getFullYear() <= year.value,
    })
  }

  date_list.push(week_days)

  watch(month, async (nMonth, oMonth) => {
    date_list.splice(0, date_list.length);
    const today = new Date();
    const first_date = new Date(year.value, nMonth - 1, 1);
    const last_date = new Date(year.value, nMonth, 0);
    count_week = Math.ceil((last_date.getDate() + first_date.getDay() - 1) / 7)
    first_monday = first_date.getDay() - 1

    let i = first_date.getDay() == 0 ? -6 : (0 - (first_date.getDay() - 1))

    let week_days = [];
    for (; i < (last_date.getDate()); i++) {
      if (week_days.length === 7) {
        date_list.push(week_days);
        week_days = [];
      }
      week_days.push({
        thisMonth: i >= 0,
        text: (i + 1),
        today: today.getDate() === (i + 1) && today.getMonth() === (nMonth - 1),
        previousDay: today.getDate() <= (i + 1) && today.getMonth() <= (nMonth - 1) && today.getFullYear() <= year.value,
      })
    }

    date_list.push(week_days)
  })

  function hello() {
    console.log("hello");
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