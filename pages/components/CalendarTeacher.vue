<template>
    <head>
        <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    </head>
  <div class="calendar-container">
    <div class="calendar-flex">
      <Datepicker :current-month="currentMonth" :current-year="currentYear" @update-date="updateCurrentDate"/>
      <div class="calendar-content">
        <div class="calendar-header">
          <div class="nav-buttons">
            <button class="nav-button prev-next-button" @click="prevMonth">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-left">
                <polyline points="15 18 9 12 15 6"></polyline>
              </svg>
            </button>
            <button class="nav-button prev-next-button" @click="nextMonth">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-right">
                <polyline points="9 18 15 12 9 6"></polyline>
              </svg>
            </button>
            <button class="nav-button today-button" @click="goToToday">Today</button>
          </div>
        </div>
        <div class="calendar-body">
          <div class="calendar-weekdays">
            <div v-for="(day, index) in weekdays" :key="index" :class="{ 'weekend': index === 0 || index === 6 }" class="weekday">
              {{ day }}
            </div>
          </div>
          <div class="calendar-dates">
            <div
              v-for="(date, index) in dates"
              :key="index"
              :class="['date', { 'not-current-month': !date.isCurrentMonth, 'today': isToday(date), 'weekend': isWeekend(date) }]"
              @click="selectDate(date)"
            >
              <span>{{ date.day }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Datepicker from '/pages/components/Datepicker.vue';

const currentDate = ref(new Date());
const weekdays = ['อาทิตย์', 'จันทร์', 'อังคาร', 'พุธ', 'พฤหัสบดี', 'ศุกร์', 'เสาร์'];

const currentYear = computed(() => currentDate.value.getFullYear());
const currentMonth = computed(() => currentDate.value.getMonth());
const currentMonthName = computed(() => currentDate.value.toLocaleString('th-TH', { month: 'long' }));

const generateDates = () => {
  const daysInCurrentMonth = new Date(currentYear.value, currentMonth.value + 1, 0).getDate();
  const firstDayIndex = new Date(currentYear.value, currentMonth.value, 1).getDay();

  const prevMonthDates = [...Array(firstDayIndex)].map((_, i) => ({
    day: new Date(currentYear.value, currentMonth.value - 1, -firstDayIndex + i + 1).getDate(),
    isCurrentMonth: false,
  }));

  const currentMonthDates = [...Array(daysInCurrentMonth)].map((_, i) => ({
    day: i + 1,
    isCurrentMonth: true,
  }));

  const nextMonthDates = [...Array(42 - prevMonthDates.length - currentMonthDates.length)].map((_, i) => ({
    day: i + 1,
    isCurrentMonth: false,
  }));

  return [...prevMonthDates, ...currentMonthDates, ...nextMonthDates];
};

const dates = computed(() => generateDates());

const nextMonth = () => {
  currentDate.value = new Date(currentYear.value, currentMonth.value + 1, 1);
};

const prevMonth = () => {
  currentDate.value = new Date(currentYear.value, currentMonth.value - 1, 1);
};

const goToToday = () => {
  currentDate.value = new Date();
};

const isToday = (date) => {
  const today = new Date();
  return date.day === today.getDate() && currentMonth.value === today.getMonth() && currentYear.value === today.getFullYear();
};

const isWeekend = (date) => {
  const weekday = new Date(currentYear.value, currentMonth.value, date.day).getDay();
  return weekday === 0 || weekday === 6;
};

const updateCurrentDate = (date) => {
  currentDate.value = date;
};
</script>

<style scoped>
.calendar-container {
  width: 100%;
  max-width: 95%;
  margin: 0 auto;
  margin-top: 10px;
  padding: 10px;
  font-family: 'Inter', sans-serif;
  background-color: #F9F9F9;
  border-radius: 10px;
}

.calendar-flex {
  display: flex;
  justify-content: space-between;
}

.calendar-content {
  flex: 1;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.nav-buttons {
  display: flex;
  align-items: center;
}

.nav-button {
  border: none;
  padding: 6px 10px;
  cursor: pointer;
  font-size: 16px;
  background-color: white;
  color: black;
  border-radius: 20px;
  border: 1px solid #ddd;
  margin-right: 5px;
}

.prev-next-button {
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.today-button {
  font-weight: bold;
  padding: 6px 15px;
}

.current-month {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  position: relative;
}

.month {
  text-align: center;
  margin-right: 130px;
  font-size: 1.4em;
  font-weight: normal;
  color: #333;
  flex-grow: 1;
}

.year {
  font-size: 1.2em;
  font-weight: normal;
  text-align: right;
  color: #333;
}

.calendar-body {
  display: flex;
  flex-direction: column;
}

.calendar-weekdays, .calendar-dates {
  display: flex;
  flex-wrap: wrap;
}

.weekday, .date {
  width: calc(100% / 7);
  box-sizing: border-box;
  text-align: center;
  padding: 10px;
  font-weight: normal;
  font-size: 16px;
}

.weekday {
  background-color: #ffffff;
  color: #333;
}

.weekend {
  color: red;
}

.date {
  height: calc(100vh / 8);
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  border: 1px solid #ddd;
  position: relative;
}

.date span {
  position: absolute;
  top: 5px;
  left: 5px;
  color: #999;
  font-size: 14px; /* ปรับขนาดฟอนต์เป็น 14px */
}

.weekend span {
  color: red;
  font-size: 14px; /* ปรับขนาดฟอนต์เป็น 14px */
}

.not-current-month span {
  color: #ddd;
  font-size: 14px; /* ปรับขนาดฟอนต์เป็น 14px */
}

.today {
  background-color: #f0f0f0;
}

/* Style for Datepicker */
.datepicker {
  width: 300px;
  margin-left: 20px;
  margin-right: 20px;
  margin-bottom: 350px;
}

.datepicker-container {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
</style>
