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
        <div class="student-form">
          <div class="form-visit" v-if="Object.keys(formSections).length > 0">
            <FormVisit :term="term" :studentID="stuId" :sections="formSections.value" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import FormVisit from '/pages/components/FormVisit.vue';

const route = useRoute();
const stuId = route.params.stuid;
const term = route.params.term;

const formSections = reactive({});
const showFormVisit = ref(true); // ตั้งค่าให้ formVisit แสดงทันที

const fetchFormSections = async () => {
  try {
    const response = await axios.get(`http://26.250.208.152:8000/api/forms/`);
    formSections.value = response.data;
    console.log('Fetched form sections:', formSections.value); // Log the data here
  } catch (error) {
    console.error('เกิดข้อผิดพลาดในการดึงข้อมูลฟอร์ม:', error);
  }
};

fetchFormSections(); // ดึงข้อมูลฟอร์มทันที
</script>

<style scoped>
/* สไตล์เดิม */
.container {
  display: flex;
  height: 100vh;
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
  min-height: calc(75vh);

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
  background-color: #F9F9F9;
  margin-bottom: 10px;
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

.form-visit {
  max-height: calc(75vh);
  overflow-y: scroll;
}

.form-visit::-webkit-scrollbar-track {
  border-radius: 5px;
  background-color: #00000013 !important;
  /* Change the background color */
}
</style>