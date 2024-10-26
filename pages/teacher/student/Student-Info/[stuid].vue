<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div class="container">
    <SidebarTeacher />

    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">ข้อมูลนักเรียน</span>
          <router-link to="/teacher/Home-Teacher">
            <button class="back">ย้อนกลับ</button>
          </router-link>
        </div>
        <div class="button-container">
          <div class="button-group">
            <button class="toggle-button" :class="{ active: isActive === 'map' }" @click="goToMap">แผนที่บ้าน</button>
          </div>
        </div>
        <StudentDetails :isEditing="isEditing" :StuId="stuId"/>
      </div>
    </div>
  </div>
</template>

<script setup>
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import StudentDetails from '/pages/components/StudentDetails.vue';
import { useRoute, useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const route = useRoute();
const router = useRouter();
const stuId = route.params.stuid; // รับค่า stuId จาก route params
const isActive = ref('info');
const mapUrl = ref(''); // สร้าง ref สำหรับ mapUrl

// ฟังก์ชันเพื่อดึงข้อมูลนักเรียน รวมถึง map_url จากฐานข้อมูล
const fetchStudentData = async () => {
  try {
    const response = await axios.get(`http://26.250.208.152:8000/api/student/${stuId}`);
    mapUrl.value = response.data.map_url; // บันทึก map_url ลงใน ref
  } catch (error) {
    console.error('เกิดข้อผิดพลาดในการดึงข้อมูล:', error);
  }
};

// เรียกฟังก์ชันเมื่อ component ถูก mount
onMounted(() => {
  fetchStudentData();
});

const goToMap = () => {
  isActive.value = 'map';
  if (mapUrl.value) {
    window.open(mapUrl.value, '_blank');
  } else {
    console.warn('ไม่พบ URL ของแผนที่');
  }
};
</script>

<style scoped>
  .container {
    display: flex;
    height: 103vh;
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
  }

  .info-text {
    font-size: 18px;
    font-weight: 500;
    color: #333;
  }

  .back {
        background-color: #56A7F5; /* Matches the edit button color */
        color: white;
        border: none;
        font-size: 14px;
        padding: 8px 16px;
        border-radius: 10px;
        cursor: pointer;
        transition: background-color 0.3s ease;
  }

  .back:hover {
        background-color: #5bacf8; /* Hover effect color */
  }
  .button-container {
    display: flex;
    justify-content: center; /* จัดตรงกลางในแนวนอน */
    align-items: flex-start; /* ชิดด้านบน */
    padding-top: 20px; /* เพิ่มระยะห่างจากด้านบน */
    width: 100%; /* ให้คอนเทนเนอร์ครอบคลุมความกว้างเต็ม */
  }

  .button-container {
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: flex-start; /* Align to the top */
  padding-top: 20px; /* Spacing from the top */
  width: 100%; /* Full width */
}

.button-group {
  display: flex;
  gap: 30px; /* Spacing between buttons */
}

.toggle-button {
  display: inline-block;
  width: 120px;
  height: 40px;
  line-height: 40px; /* Center text vertically */
  border: none;
  border-radius: 15px; /* Rounded corners */
  font-size: 18px;
  cursor: pointer;
  outline: none;
  color: #56A7F5;
  background-color: #ECECEC;
  text-align: center; /* Center text horizontally */
  transition: background-color 0.3s ease, color 0.3s ease, box-shadow 0.3s ease;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1); /* Subtle shadow */
}

.toggle-button.active {
  background-color: #56A7F5;
  color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* More pronounced shadow for active */
}

.toggle-button:hover {
  background-color: #5bacf8; /* Light gray */
  color: white; /* Blue text */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15); /* Slightly deeper shadow on hover */
}

</style>  