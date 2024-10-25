<template>
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
            <div v-for="(day, index) in weekdays" :key="index" 
                 :class="['weekday', { 'sunday': index === 0, 'saturday': index === 6, 'weekend': index === 0 || index === 6 }]">
              {{ day }}
            </div>
          </div>
          <div class="calendar-dates">
            <div
              v-for="(date, index) in dates"
              :key="index"
              :class="['date', { 
                'not-current-month': !date.isCurrentMonth, 
                'today': isToday(date), 
                'weekend': isWeekend(date), 
                'last-week': date.lastWeek 
              }]"
              @click="selectDate(date)"
            >
              <span class="date-number">{{ date.day }}</span>
              
              <!-- Show booking names if date matches -->
              <div class="booking-names" v-if="getBookingNamesForDate(date).length">
                <span 
                  v-for="(name, nameIndex) in getBookingNamesForDate(date)" 
                  :key="nameIndex" 
                  class="event-text" 
                  :style="getRandomStyles(nameIndex)"
                >
                  {{ name }}
                </span>
              </div>
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

// Function to generate random color
const getRandomColor = () => {
  const r = Math.floor(Math.random() * 128); // จำกัดค่าไม่เกิน 128
  const g = Math.floor(Math.random() * 128);
  const b = Math.floor(Math.random() * 128);
  return `rgb(${r}, ${g}, ${b})`;
};

// Function to get a lighter version of the color
const getLighterColor = (color) => {
  const rgb = color.match(/\d+/g).map(Number);
  const r = Math.min(255, Math.floor(rgb[0] + (255 - rgb[0]) * 0.5)); // Adjust brightness
  const g = Math.min(255, Math.floor(rgb[1] + (255 - rgb[1]) * 0.5)); 
  const b = Math.min(255, Math.floor(rgb[2] + (255 - rgb[2]) * 0.5)); 
  return `rgb(${r}, ${g}, ${b})`;
};



// Function to get random styles for names
const getRandomStyles = (index) => {
  const baseColor = getRandomColor(); // Generate a random color
  return {
    backgroundColor: getLighterColor(baseColor), // Lighter color for background
    color: baseColor, // Darker color for text
    marginRight: '5px' // Optional spacing between names
  };
};

const currentDate = ref(new Date());
const weekdays = ['อาทิตย์', 'จันทร์', 'อังคาร', 'พุธ', 'พฤหัสบดี', 'ศุกร์', 'เสาร์'];

const currentYear = computed(() => currentDate.value.getFullYear());
const currentMonth = computed(() => currentDate.value.getMonth());
const currentMonthName = computed(() => currentDate.value.toLocaleString('th-TH', { month: 'long' }));

const bookings = ref([
  { date: '2024-10-04', names: ['นางสาวxxx xxx'] },
  { date: '2024-10-04', names: ['นางสาว123 123'] },
  { date: '2024-10-10', names: ['นายxxx xxx'] },
  { date: '2024-10-15', names: ['นางสาวxxx xxx'] },
  { date: '2024-10-15', names: ['นายxxx xxx'] }
]);

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
    lastWeek: (i >= (daysInCurrentMonth - 2)) // Check if it's the last week of the month
  }));

  const totalDisplayedDates = 5 * 7; // 5 rows x 7 days
  const nextMonthDatesCount = totalDisplayedDates - (prevMonthDates.length + currentMonthDates.length);
  const nextMonthDates = [...Array(nextMonthDatesCount)].map((_, i) => ({
    day: i + 1,
    isCurrentMonth: false,
  }));

  return [...prevMonthDates, ...currentMonthDates, ...nextMonthDates].slice(0, totalDisplayedDates);
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

// Function to get booking names for the specific date
const getBookingNamesForDate = (date) => {
  const dateString = `${currentYear.value}-${String(currentMonth.value + 1).padStart(2, '0')}-${String(date.day).padStart(2, '0')}`;
  return bookings.value
    .filter(b => b.date === dateString) // Filter bookings by date
    .flatMap(b => b.names); // Combine names into a single array
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
  margin-top: 10px;
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
  color: #000000;
  justify-content: center;
  align-items: center;
}

.today-button {
  font-weight: bold;
  font-size: 14px;
  padding: 6px 15px;
  color: #000000;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
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
  padding: 10px;
  font-weight: normal;
  font-size: 16px;
}

.weekday {
  text-align: left;
  color: #676767;
  font-size: 14px;
  border: 1px solid #ddd;
}

.sunday {
  border-top-left-radius: 10px;
}

.saturday {
  border-top-right-radius: 10px;
}

.weekend {
  color: red;
}

.weekend span {
  color: red;
  font-size: 14px;
}

.date {
  position: relative;
  height: calc(100vh / 8);
  display: flex;
  flex-direction: column; /* ให้ชิดขอบล่าง */
  justify-content: space-between; /* ให้วันที่อยู่มุมซ้ายบน */
  border: 1px solid #ddd;
}

.date-number {
  position: absolute;
  top: 5px; /* ตำแหน่งวันที่ */
  left: 5px;
  color: #969696;
  font-size: 14px;
}

.booking-names {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin-top: auto; /* ชิดขอบล่าง */
  margin-right: 15px;
}

.event-text {
  font-size: 11px;
  padding: 3px 3px; /* ระยะห่างภายใน */
  border-radius: 3px; /* ขอบมน */
  display: inline-block; /* ให้พื้นหลังครอบคลุมข้อความ */
  margin-bottom: 2px; /* เพิ่มระยะห่างระหว่างชื่อ */
}
</style>
