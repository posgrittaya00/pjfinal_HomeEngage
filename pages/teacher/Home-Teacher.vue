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

        <div class="search-container">
          <i class="search-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-search"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
          </i>
          <input
            type="text"
            v-model="searchQuery"
            placeholder="ค้นหา"
            class="search-input"
            @input="updateFilteredStudents"
          />
        </div>

        <!-- แสดงรายชื่อนักเรียน -->
        <ProfileStudentTeacher
          :students="paginatedStudents"
          :currentPage="currentPage"
          :studentsPerPage="studentsPerPage"
        />

        <!-- Pagination -->
        <Pagination 
          :currentPage="currentPage" 
          :totalPages="totalPages" 
          @set-page="setPage" 
          @prev="prevPage" 
          @next="nextPage" 
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import ProfileStudentTeacher from '../components/ProfileStudent-Teacher.vue';
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import Pagination from '../components/Pagination.vue';
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';

const isLoading = ref(true);
const hasError = ref(false);
const errorMessage = ref('');
const students = ref([]);
const filteredStudents = ref([]);
const searchQuery = ref("");
const studentsPerPage = 10;
const currentPage = ref(1);

onMounted(async () => {
  try {
    const username = localStorage.getItem('username');
    const response = await axios.get(`http://26.250.208.152:8000/api/teacher/students/${username}`);
    students.value = response.data;
    filteredStudents.value = students.value;  // กำหนดค่าเริ่มต้นให้กับ filteredStudents
    console.log("Initial Students Data:", students.value); // ตรวจสอบข้อมูลนักเรียนที่ได้รับ
  } catch (error) {
    console.error("Error fetching student data:", error.message);
    hasError.value = true;
    errorMessage.value = error.message || "เกิดข้อผิดพลาดในการดึงข้อมูลนักเรียน";
  } finally {
    isLoading.value = false;
  }
});

// ฟังก์ชันสำหรับกรองนักเรียน
const updateFilteredStudents = () => {
  currentPage.value = 1;
  const query = searchQuery.value.toLowerCase();

  filteredStudents.value = students.value.filter(student => {
    // ตรวจสอบว่า student มีฟิลด์ที่ต้องการทั้งหมด
    const stuId = student.stu_id ? student.stu_id.toString().toLowerCase() : "";
    const name = student.name ? student.name.toLowerCase() : "";
    const stuClass = student.stu_class ? student.stu_class.toLowerCase() : "";

    // ทำการกรองโดยใช้ฟิลด์ที่แปลงค่าไว้
    return stuId.includes(query) || name.includes(query) || stuClass.includes(query);
  });

  console.log("Filtered Students after search:", filteredStudents.value); // Log ผลลัพธ์การกรอง
};

// คำนวณรายชื่อนักเรียนที่อยู่ในหน้านั้นๆ
const paginatedStudents = computed(() => {
  const start = (currentPage.value - 1) * studentsPerPage;
  const end = start + studentsPerPage;
  return filteredStudents.value.slice(start, end);
});

// คำนวณจำนวนหน้าทั้งหมด
const totalPages = computed(() => {
  return Math.ceil(filteredStudents.value.length / studentsPerPage);
});

// ฟังก์ชันสำหรับจัดการการเปลี่ยนหน้า
const setPage = (page) => {
  currentPage.value = page;
};

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};
</script>

<style scoped>
  .container {
    display: flex;
    height: 128vh;
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
    position: relative;
    display: flex;
    flex-direction: column;  
    flex: 1;
    background-color: #F9F9F9;
    margin: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding-bottom: 60px; 
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

  .pagination {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    padding: 10px 0;
    background-color: #F9F9F9; 
    margin-bottom: 10px; 
  }
</style>