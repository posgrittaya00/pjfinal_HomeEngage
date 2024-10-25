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
            placeholder="Search"
            class="search-input"
          />
        </div>

        <!-- Show student list -->
        <ProfileStudentTeacher
          :students="filteredStudents"
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
import { ref, computed } from 'vue';
import axios from 'axios';

const isLoading = ref(true);
    const hasError = ref(false);
    const errorMessage = ref('');

    onMounted(async () => {
      try {
        const username = localStorage.getItem('username');

        if (username) {
          const response = await axios.get(`http://26.250.208.152:8000/api/teacher/students/${username}`);
          
          if (response && response.data) {
            students.value = response.data;  // Assign the fetched data
              // Emit the data to parent
          } else {
            throw new Error("Invalid student data");
          }
        } else {
          throw new Error("No username found in localStorage");
        }
      } catch (error) {
        console.error("Error fetching student data:", error.message);
        hasError.value = true;
        errorMessage.value = error.message || "An error occurred while fetching student data.";
      } finally {
        isLoading.value = false;
      }
    });

    // return {
    //   // formData,
    //   isLoading,
    //   hasError,
    //   errorMessage,
    // };

const studentsPerPage = 10;
const currentPage = ref(1);
const searchQuery = ref("");

  // ข้อมูลนักเรียนทั้งหมด
  const students = ref([
 

]);

const filteredStudents = computed(() => {
  const query = searchQuery.value.toLowerCase();
  const filtered = students.value.filter(student => 
    (student.Name || '').toLowerCase().includes(query) ||
    (student.StuId || '').toLowerCase().includes(query) ||
    (student.StuClass || '').toLowerCase().includes(query) // เพิ่มเงื่อนไขสำหรับ StuClass
  );
  const start = (currentPage.value - 1) * studentsPerPage;
  return filtered.slice(start, start + studentsPerPage);
});

const totalPages = computed(() => {
  const query = searchQuery.value.toLowerCase();
  const filtered = students.value.filter(student => 
    (student.Name || '').toLowerCase().includes(query) ||
    (student.StuId || '').toLowerCase().includes(query) ||
    student.StuClass.toLowerCase().includes(query) // เพิ่มเงื่อนไขสำหรับ StuClass
  );
  return Math.ceil(filtered.length / studentsPerPage);
});

// Function to set the page
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
</style>