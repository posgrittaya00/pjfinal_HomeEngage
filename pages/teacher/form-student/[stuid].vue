<template>
  <div class="container">
    <SidebarTeacher />

    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">ข้อมูลการเยี่ยมบ้านนักเรียน</span>
        </div>
        <div class="student-form" >
          <StudentDetails :StuId="stuId" v-if="!showFormVisit" />
          <div class="form-visit" v-else >
          <FormVisit :sections="formSections.value" v-if="Object.keys(formSections).length > 0" />
        </div>
        </div>

        <!-- ปุ่มแสดงฟอร์มเมื่อกด -->
        <div v-if="!showFormVisit" class="next-button-container">
          <button class="next-button" @click="goToFormVisit">ต่อไป</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive  } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import StudentDetails from '/pages/components/StudentDetails.vue';
import FormVisit from '/pages/components/FormVisit.vue'; // เพิ่มการนำเข้า FormVisit

const route = useRoute();
const stuId = route.params.stuid;

// สร้าง state เพื่อเก็บข้อมูล sections ของฟอร์ม
const formSections = reactive({});
const showFormVisit = ref(false); // สร้างตัวแปรเพื่อควบคุมการแสดงฟอร์ม

// ฟังก์ชันดึงข้อมูล sections ของฟอร์ม
const fetchFormSections = async () => {
  try {
    const response = await axios.get(`http://26.250.208.152:8000/api/forms/`);
    formSections.value = response.data; 
    console.log('Fetched form sections:', formSections.value); // Log the data here
  } catch (error) {
    console.error('เกิดข้อผิดพลาดในการดึงข้อมูลฟอร์ม:', error);
  }
};

// ฟังก์ชันสำหรับการกดปุ่ม "ต่อไป" และดึงข้อมูลฟอร์ม
const goToFormVisit = async () => {
  try {
    await fetchFormSections(); // ดึงข้อมูลฟอร์มหลังจากกดปุ่ม
    showFormVisit.value = true; // แสดงฟอร์ม
  } catch (error) {
    console.error(error)
  }
};

</script>

  <style scoped>
    /* สไตล์เดิม */
    .container {
      display: flex;
      height: 120vh;
      font-family: 'Inter', sans-serif;
    }

    .main-content {
      flex: 1;
      display: flex;
      flex-direction: column;
      background-color: #f5f5f5;
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
      position: relative; /* ตั้งตำแหน่ง relative เพื่อให้ absolute ด้านในอ้างอิง */
      display: flex;
      flex-direction: column;  /* จัดเรียงแนวตั้ง */
      flex: 1;
      background-color: #F9F9F9;
      margin: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      padding-bottom: 60px; /* เพิ่มระยะห่างจากขอบล่าง */
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
    }

    .info-text {
      font-size: 18px;
      font-weight: 500;
      color: #333;
    }

    .search-container {
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 25px auto;
      position: relative;
      width: 400px;
      height: 40px;
      border: 1px solid #c2c2c2;
      border-radius: 20px;
      background-color: #F9F9F9;
    }

    .search-icon {
      margin-left: 10px;
      color: #b0b0b0;
    }

    .search-input {
      width: 100%;
      border: none;
      outline: none;
      padding: 8px 12px;
      font-size: 16px;
      border-radius: 20px;
    }

    .search-input::placeholder {
      color: #b0b0b0;
    }

    /* ปรับตำแหน่ง Pagination ให้ชิดขอบล่าง */
    .pagination {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      padding: 10px 0;
      background-color: #F9F9F9; /* เพื่อให้ดูชัดเจนเมื่อชิดขอบล่าง */
      margin-bottom: 10px; /* เพิ่มระยะห่างระหว่างตารางและ Pagination */
    }
    .next-button-container {
      display: flex;
      justify-content: center;
      font-family: 'Inter', sans-serif;
    }
    .next-button {
      padding: 10px 20px;
      font-size: 18px;
      background-color: #56A7F5;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    .next-button:hover {
      background-color: #3b8fd3;
    }

  </style>