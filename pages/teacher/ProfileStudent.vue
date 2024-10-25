<template>
  <div class="container">
    <SidebarTeacher />

    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">เพิ่มข้อมูลการเยี่ยมบ้านนักเรียน</span>
          <button class="save-button" @click="saveData">บันทึกข้อมูล</button>
          <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
        </div>
        <StudentDetailsTeacher
          class="student-details"
          @updateData="handleUpdateData"
          ref="studentDetails"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import StudentDetailsTeacher from '../components/StudentDetails-Teacher.vue';
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import { ref } from 'vue';
import axios from 'axios';

const handleUpdateData = (data) => {
  // Handle updated data if necessary
};

const successMessage = ref('');

const saveData = async () => {
  const studentDetails = ref(null);
  try {
    studentDetails.value = await $refs.studentDetails.formData;
    const response = await axios.post('http://26.250.208.152:8000/api/student', studentDetails.value);
    if (response.status === 200) {
      successMessage.value = 'บันทึกข้อมูลสำเร็จ!'; // ตั้งค่าข้อความสำเร็จ
      setTimeout(() => {
        successMessage.value = ''; // เคลียร์ข้อความหลังจาก 3 วินาที
      }, 3000);
    } else {
      alert('เกิดข้อผิดพลาดในการบันทึกข้อมูล');
    }
  } catch (error) {
    console.error('Error saving data:', error);
    alert('เกิดข้อผิดพลาด: ' + error.message);
  }
};
</script>

<style scoped>
  .container {
  display: flex;
  height: 105vh; /* ปรับขนาดเป็น 100% ของหน้าจอ */
  font-family: 'Inter', sans-serif;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
  min-height: 100vh; /* ปรับขนาดให้เต็มหน้าจอ */
}

  .header {
    background-color: #56A7F5;
    padding: 5px 20px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    height: 80px;
  }

  .header h1 {
    font-size: 42px;
    font-weight: 400;
    color: white;
    font-family: "Chewy", system-ui;
    margin-left: 20px;
  }

  .content-area {
    flex: 1;
    background-color: #F9F9F9;
    margin: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  .info-box {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #ECECEC;
    border-radius: 8px 8px 0 0;
    padding: 18px 20px;
    font-size: 18px;
    color: #333;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 10px;
  }

  .info-text {
    font-size: 18px;
    font-weight: 500;
    color: #333;
  }

  .student-details {
    flex-grow: 1; /* ทำให้ StudentDetails ขยายตัวให้เต็มพื้นที่ */
  }

  .save-button {
    background-color: #55c058;
    color: white;
    border: none;
    padding: 10px 16px;
    border-radius: 10px;
    font-size: 14px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .save-button:hover {
    background-color: #69b76b;
  }

  .pagination {
  position: relative; /* เปลี่ยนจาก absolute เพื่อไม่ให้ยึดติดกับด้านล่าง */
  display: flex;
  justify-content: center;
  padding: 5px 0; /* ลด padding */
  background-color: #F9F9F9;
  margin-bottom: 5px; /* ลดระยะห่างระหว่างตารางและ Pagination */
}
  .success-message {
    color: #55c058; /* สีเขียวสำหรับข้อความสำเร็จ */
    margin-top: 10px; /* ระยะห่างจากปุ่ม */
    font-weight: bold; /* ข้อความตัวหนา */
  }
</style>