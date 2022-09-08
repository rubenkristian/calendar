<script setup>
  import { ref } from 'vue'
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Calendar from './components/Calendar.vue'

const months = [
  {display: "Jan", value: 1}, 
  {display: "Feb", value: 2}, 
  {display: "Mar", value: 3}, 
  {display: "Apr", value: 4}, 
  {display: "May", value: 5}, 
  {display: "Jun", value: 6}, 
  {display: "Jul", value: 7}, 
  {display: "Agu", value: 8}, 
  {display: "Sep", value: 9}, 
  {display: "Oct", value: 10}, 
  {display: "Nov", value: 11}, 
  {display: "Dec", value: 12}
];

const today = new Date();

let selected_month = ref(today.getMonth() + 1);

function selectMonth(e) {
  selected_month.value = Number(e.target.value);
}
</script>

<template>
  <div>
    <select @change="selectMonth">
      <option v-for="month in months" :value="month.value" :selected="month.value === selected_month">
        {{ month.display }}
      </option>
    </select>
    <Calendar :year="2022" :month="selected_month"/>
  </div>
</template>

<style scoped>
</style>
