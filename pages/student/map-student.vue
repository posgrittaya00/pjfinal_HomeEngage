<template>
    <Head>
      <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    </Head>
    <div class="container">
      <div class="sidebar">
        <div class="profile-section">
          <div class="profile-image">
            <img :src="user.profileImage" alt="Profile Image" />
          </div>
          <p class="profile-name">{{ user.name }}</p>
          <p class="profile-detail">{{ user.studentClass }} รหัสประจำตัว {{ user.studentId }}</p>
        </div>
        <div class="menu">
          <router-link to="/student/home-student" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg></i>
            <span>หน้าแรก</span>
          </router-link>
          <router-link to="/student/profile-student" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-user"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg></i>
            <span>โปรไฟล์</span>
          </router-link>
          <router-link to="/booking" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg></i>
            <span>จองวันเยี่ยมบ้าน</span>
          </router-link>
        </div>
        <div class="logout-section">
          <button class="logout-button" @click="logout">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-log-out"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path><polyline points="16 17 21 12 16 7"></polyline><line x1="21" y1="12" x2="9" y2="12"></line></svg></i>
            <span>Logout</span>
          </button>
        </div>
      </div>
      
      <div class="main-content">
        <div class="header">
          <h1>HOME ENGAGE</h1>
        </div>
        <div class="content-area">
          <div class="info-box">
                <span class="info-text">{{ user.name }}</span>
                <button class="edit-button">แก้ไข</button>
            </div>
            <div class="button-container">
              <div class="button-group">
                <button class="toggle-button" :class="{ active: isActive === 'info' }" @click="goToProfile">ข้อมูล</button>
                <button class="toggle-button" :class="{ active: isActive === 'map' }" @click="goToMap">แผนที่บ้าน</button>
              </div>
            </div>
              <div class="map-url-container">
                  <!-- Input for Google Map URL -->
                  <input
                    v-model="mapInput"
                    placeholder="Google map URL"
                  />

                  <!-- Button to update the map URL -->
                  <button @click="updateMapUrl">
                    อัพเดตแผนที่
                  </button>
                </div>

                <!-- Iframe to display the map -->
                <iframe
                  v-if="mapUrl"
                  :src="mapUrl"
                  allowfullscreen=""
                  loading="lazy"
                  referrerpolicy="no-referrer-when-downgrade">
                </iframe>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

// ข้อมูลของผู้ใช้งาน
const user = {
  profileImage: '/images/User.png',
  name: "นาย xxx xxx",
  studentClass: "ชั้นม.5/1",
  studentId: "xxxxxxxxx-x"
};

// การจัดการ router
const router = useRouter();

const isActive = ref('map'); 

const goToMap = () => {
  isActive.value = 'map'; // เปลี่ยนสถานะเป็นแผนที่
  router.push('/student/map-student'); // เปลี่ยนไปที่หน้า map-student
};

const goToProfile = () => {
  isActive.value = 'info'; // เปลี่ยนสถานะเป็นข้อมูล
  router.push('/student/profile-student'); // เปลี่ยนไปที่หน้า profile-student
};


// ฟังก์ชัน logout
const logout = () => {
  console.log("Logging out...");
  router.push('/');
};

const mapInput = ref('');
const mapUrl = ref('');

const updateMapUrl = () => {
  const url = mapInput.value.trim();
  let lat, lng;

  // รูปแบบที่ใช้ดึงพิกัด
  const patterns = [
    /@(-?\d+\.\d+),(-?\d+\.\d+)/,   // @latitude,longitude
    /!3d(-?\d+\.\d+)!4d(-?\d+\.\d+)/,  // !3dlatitude!4dlongitude
    /q=(-?\d+\.\d+),(-?\d+\.\d+)/,    // q=latitude,longitude
    /maps.google.com\/maps\?q=(-?\d+\.\d+),(-?\d+\.\d+)/ // maps.google.com/?q=latitude,longitude
  ];

  for (let pattern of patterns) {
    const match = url.match(pattern);
    if (match) {
      lat = match[1];
      lng = match[2];
      break;
    }
  }

  // การจัดการกรณีลิงก์ย่อ
  if (!lat && url.includes('maps.app.goo.gl')) {
    alert('กรุณาคัดลอกพิกัดจากลิงก์ย่อที่เปิดในเบราว์เซอร์');
    return;
  }

  if (lat && lng) {
    // สร้าง URL สำหรับแสดงแผนที่
    mapUrl.value = `https://maps.google.com/maps?q=${lat},${lng}&output=embed`;
  } else {
    alert('ไม่สามารถดึงพิกัดจาก URL ที่กรอกได้');
  }
};
</script>

  <style scoped>
.container {
    display: flex;
    height: 111vh;
    font-family: 'Inter', sans-serif;
  }
  
  .sidebar {
    width: 280px;
    background-color: #fff;
    display: flex;
    flex-direction: column;
    padding: 0;
    border-right: 1px solid #e0e0e0;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
    font-family: 'Inter', sans-serif;
  }
  
  .profile-section {
    text-align: center;
    padding: 30px 0;
    border-bottom: 1px solid #e0e0e0;
  }
  
  .profile-image img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-color: #f0f0f0;
    object-fit: cover;
  }
  
  .profile-name {
    margin-top: 10px;
    font-size: 18px;
    font-weight: 500;
    color: #333;
  }
  
  .profile-detail {
    font-size: 14px;
    color: #888;
    margin-top: 5px;
    margin-bottom: 0;
  }
  
  .menu {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    margin-top: 10px;
  }

  .menu-span {
    font-family: 'Inter', sans-serif;
    font-size: 16px; /* กำหนดขนาดฟอนต์ */
    color: #333; /* สีตัวหนังสือ */
    margin-left: 10px; /* ระยะห่างจากไอคอน */
  }
  
  .menu-item {
    display: flex;
    align-items: center;
    padding: 12px 25px;
    font-size: 16px;
    color: #333;
    text-decoration: none;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  
  .menu-item i {
    margin-right: 15px;
    color: #56A7F5;
    font-size: 18px;
  }
  
  .menu-item span {
    flex: 1;
  }
  
  .menu-item:hover {
    background-color: #f9f9f9;
    color: #56A7F5;
  }
  
  .logout-section {
    border-top: 1px solid #e0e0e0;
    padding: 10px 0;
    text-align: center;
  }
  
  .logout-button {
    background-color: #ff6b6b;
    color: white;
    border: none;
    padding: 12px 25px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 25px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease;
    width: 80%;
    margin: 0 auto;
    margin-top: 10px;
  }
  
  .logout-button i {
    margin-right: 10px;
    font-size: 16px;
  }
  
  .logout-button:hover {
    background-color: #ff5252;
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
    background-color: #ECECEC; /* Adjust this color to match the background */
    border-radius: 8px 8px 0 0; /* มุมมนเฉพาะด้านบน */
    padding: 12px 20px;
    font-size: 16px;
    color: #333;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .info-text {
        font-size: 18px;
        font-weight: 500;
        color: #333;
  }

  .edit-button {
        background-color: #ff6b6b; /* Matches the edit button color */
        color: white;
        border: none;
        padding: 8px 16px;
        border-radius: 10px;
        cursor: pointer;
        transition: background-color 0.3s ease;
  }

  .edit-button:hover {
        background-color: #ff5252; /* Hover effect color */
  }

  .button-container {
    display: flex;
    justify-content: center; /* จัดตรงกลางในแนวนอน */
    align-items: flex-start; /* ชิดด้านบน */
    padding-top: 20px; /* เพิ่มระยะห่างจากด้านบน */
    width: 100%; /* ให้คอนเทนเนอร์ครอบคลุมความกว้างเต็ม */
  }

  .button-group {
    display: flex;
    gap: 30px; /* ระยะห่างระหว่างปุ่ม */
  }

  .toggle-button {
  text-decoration: none;
  display: inline-block;
  width: 120px;
  height: 40px;
  line-height: 40px; /* จัดข้อความให้อยู่กลางแนวตั้ง */
  border: none;
  border-radius: 15px;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
  outline: none;
  color: #56A7F5;
  background-color: #ECECEC;
  text-align: center; /* จัดข้อความให้อยู่กลางแนวนอน */
}

.toggle-button.active {
  background-color: #56A7F5;
  color: white;
}

.toggle-button:hover {
  opacity: 0.9;
}

  .toggle-button:not(.active) {
    background-color: #ECECEC; /* สีเทาเมื่อไม่ได้เลือก */
    color: #56A7F5; /* ตัวหนังสือสีฟ้า */
  }

  /* Container for input and button, centered at the top */
.map-url-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin: 20px auto; /* Center the container horizontally */
  max-width: 600px; /* Optional: Limit the width */
}

/* Style for the Google Map URL input */
.map-url-container input {
  flex: 1;
  max-width: 600px; /* Limit the max-width */
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Style for the Update button */
.map-url-container button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #66b2f9;
  color: #fff;
  border: none;
  border-radius: 10px;
  transition: background-color 0.3s;
}

.map-url-container button:hover {
  background-color: #4aa5fa;
}

.map-url-container button:active {
  background-color: #4aa5fa;
}
/* Style for the iframe map display */
iframe {
  width: 100%;
  max-width: 1050px;
  height: 450px;
  border: none;
  border-radius: 10px;
  margin-top: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: block;
  margin-left: auto;
  margin-right: auto;
}
  </style>
