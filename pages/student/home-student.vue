<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div class="container">
    <Sidebar />
    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="button-container">
          <router-link to="/student/Map-student" class="custom-button">เพิ่มข้อมูลที่อยู่</router-link>
          <router-link to="/student/Booking-Student" class="custom-button">จองวันเยี่ยมบ้าน</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Sidebar from '/pages/components/Sidebar.vue'
</script>

<style scoped>
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
  flex: 1;
  background-color: #EAE5E5;
  margin: 20px;
  border-radius: 10px;
  background-image: url('public/images/background_home_student.png'); /* Correct path for image */
  background-size: cover; /* Cover the area */
  background-position: center; /* Center the image */
}
.button-container {
  display: flex;
  flex-direction: row; /* Align buttons side by side */
  justify-content: center; /* Center the buttons horizontally */
  margin-top: 250px;
}
.custom-button {
  background-color: #EEF5F9; /* Background color */
  color: #56A7F5; /* Text color */
  font-size: 20px;
  font-family: 'Chewy', sans-serif;
  padding: 15px 30px;
  margin: 10px;
  text-decoration: none; /* Remove underline from links */
  display: inline-block; /* Make it behave like a block for padding */
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Optional shadow */
}
.custom-button:hover {
  background-color: #d1e3f9; /* Lighter shade for hover effect */
}
</style>
