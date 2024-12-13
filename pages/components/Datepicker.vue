<template>
    <head>
        <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    </head>
  <div class="calendar datepicker">
    <div class="calendar-header">
      <h2>{{ monthNames[currentMonth] }} {{ currentYear }}</h2>
    </div>
    <div class="calendar-grid">
      <div class="day" v-for="day in weekDays" :key="day">{{ day }}</div>
      <div
        class="day"
        v-for="date in datesInMonth"
        :key="date"
        :class="{ 'today': date && isSameDate(date, new Date()) }"
        @click="selectDate(date)"
      >
        {{ date ? date.getDate() : '' }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currentMonth: {
      type: Number,
      required: true,
    },
    currentYear: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      currentDate: null,
      weekDays: ['อา', 'จ', 'อ', 'พ', 'พฤ', 'ศ', 'ส'],
      monthNames: [
        'มกราคม', 'กุมภาพันธ์', 'มีนาคม', 'เมษายน', 'พฤษภาคม', 'มิถุนายน',
        'กรกฎาคม', 'สิงหาคม', 'กันยายน', 'ตุลาคม', 'พฤศจิกายน', 'ธันวาคม'
      ],
    };
  },
  computed: {
    datesInMonth() {
      const dates = [];
      const firstDayOfMonth = new Date(this.currentYear, this.currentMonth, 1);
      const lastDayOfMonth = new Date(this.currentYear, this.currentMonth + 1, 0);
      
      // เติมช่องว่างก่อนวันแรกของเดือน
      for (let i = 0; i < firstDayOfMonth.getDay(); i++) {
        dates.push(null);
      }

      // เติมวันที่ในเดือนตามจำนวนวันจริงในเดือนนั้น ๆ
      for (let i = 1; i <= lastDayOfMonth.getDate(); i++) {
        dates.push(new Date(this.currentYear, this.currentMonth, i));
      }

      // เติมช่องว่างหลังจากวันสุดท้ายของเดือนเพื่อให้เต็มสัปดาห์
      const remainingDays = 7 - (dates.length % 7);
      if (remainingDays < 7) {
        for (let i = 0; i < remainingDays; i++) {
          dates.push(null);
        }
      }

      return dates;
    },
  },
  methods: {
    prevMonth() {
      this.$emit('update-date', new Date(this.currentYear, this.currentMonth - 1, 1));
    },
    nextMonth() {
      this.$emit('update-date', new Date(this.currentYear, this.currentMonth + 1, 1));
    },
    selectDate(date) {
      if (date && !this.isSameDate(date, new Date())) {
        this.currentDate = date;
        console.log('วันที่ที่เลือก:', date);
      } else {
        console.log('ไม่สามารถเลือกวันปัจจุบันได้');
      }
    },
    isSameDate(date1, date2) {
      return date1 && date2 && 
        date1.getDate() === date2.getDate() && 
        date1.getMonth() === date2.getMonth() && 
        date1.getFullYear() === date2.getFullYear();
    },
  },
};
</script>

<style scoped>
.calendar {
  width: 100%;
  max-width: 250px; /* ลดความกว้างสูงสุดของปฏิทิน */
  margin-right: 30px;
  font-family: 'Inter', sans-serif;
}
.calendar-header {
  font-weight: bold;
  font-size: 14px; /* ลดขนาดฟอนต์ของหัวเรื่อง */
  text-align: left;
  padding-left: 8px; /* ลด padding */
  margin-bottom: 0;
}
.calendar-grid {
  display: grid;
  font-size: 12px; /* ลดขนาดฟอนต์ของวัน */
  grid-template-columns: repeat(7, 1fr);
  gap: 4px; /* ลดระยะห่างระหว่างวัน */
  margin-top: 5px;
}
.day {
  padding: 8px; /* ลด padding ของวัน */
  text-align: center;
  box-sizing: border-box;
  height: 40px; /* ลดความสูงของแต่ละวัน */
  display: flex;
  justify-content: center;
  align-items: center;
}
.day.today {
  background-color: #56A7F5;
  color: white;
  border-radius: 50%;
  font-weight: bold;
  width: 30px; /* ลดขนาดของวงกลมสำหรับวันปัจจุบัน */
  height: 30px; /* ลดความสูงของวงกลมสำหรับวันปัจจุบัน */
  margin: auto;
}
</style>