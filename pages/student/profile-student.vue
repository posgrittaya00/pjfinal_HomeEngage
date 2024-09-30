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
            <span class="menu-text">หน้าแรก</span>
          </router-link>
          <router-link to="/student/profile-student" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-user"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg></i>
            <span class="menu-text">โปรไฟล์</span>
          </router-link>
          <router-link to="/booking" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg></i>
            <span class="menu-text">จองวันเยี่ยมบ้าน</span>
          </router-link>
        </div>
        <div class="logout-section">
          <button class="logout-button" @click="logout">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-log-out"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path><polyline points="16 17 21 12 16 7"></polyline><line x1="21" y1="12" x2="9" y2="12"></line></svg></i>
            <span class="menu-text">Logout</span>
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
            <button class="edit-button" :class="{ 'save-button': isEditing }" @click="toggleEdit">
              {{ isEditing ? 'บันทึก' : 'แก้ไข' }}
            </button>
          </div>
            <div class="button-container">
              <div class="button-group">
                <button class="toggle-button" :class="{ active: isActive === 'info' }" @click="goToProfile">ข้อมูล</button>
                <button class="toggle-button" :class="{ active: isActive === 'map' }" @click="goToMap">แผนที่บ้าน</button>
              </div>
            </div>
            
            <div class="student-details">
                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล</label>
                        <span class="detail-span">{{ user.name }}</span>
                    </div>
                    <div class="field">
                        <label>ชื่อเล่น</label>
                        <span class="detail-span">{{ user.nickName }}</span>
                    </div>
                    <div class="field">
                        <label>รหัสประจำตัว</label>
                        <span class="detail-span">{{ user.studentId }}</span>
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span">{{ user.studentPhone }}</span>
                    </div>
                    <div class="field">
                        <label>ระดับชั้น</label>
                        <span class="detail-span">{{ user.studentClass }}</span>
                    </div>
                    <div class="field">
                        <label>เกิดวันที่</label>
                        <span class="detail-span">{{ user.studentBirthDate }}</span>
                    </div>
                </div>

                <div class="row">
                    <div class="field full-width">
                        <label>ที่อยู่</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.address }}</span>
                        <input v-if="isEditing" v-model="formData.address" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระยะทางจากบ้านมาโรงเรียน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.distance }}</span>
                        <input v-if="isEditing" v-model="formData.distance" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เดินทางมาโรงเรียนโดย</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.transport }}</span>
                        <input v-if="isEditing" v-model="formData.transport" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ความสามารถพิเศษ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.skills }}</span>
                        <input v-if="isEditing" v-model="formData.skills" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (บิดา)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherName }}</span>
                        <input v-if="isEditing" v-model="formData.fatherName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherJob }}</span>
                        <input v-if="isEditing" v-model="formData.fatherJob" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherPhone }}</span>
                        <input v-if="isEditing" v-model="formData.fatherPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherSalary }}</span>
                        <input v-if="isEditing" v-model="formData.fatherSalary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherEducation }}</span>
                        <input v-if="isEditing" v-model="formData.fatherEducation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (มารดา)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherName }}</span>
                        <input v-if="isEditing" v-model="formData.motherName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherJob }}</span>
                        <input v-if="isEditing" v-model="formData.motherJob" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherPhone }}</span>
                        <input v-if="isEditing" v-model="formData.motherPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherSalary }}</span>
                        <input v-if="isEditing" v-model="formData.motherSalary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherEducation }}</span>
                        <input v-if="isEditing" v-model="formData.motherEducation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (ผู้ปกครอง)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.guardianName }}</span>
                        <input v-if="isEditing" v-model="formData.guardianName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เกี่ยวข้องเป็น</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.relation }}</span>
                        <input v-if="isEditing" v-model="formData.relation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.guardianPhone }}</span>
                        <input v-if="isEditing" v-model="formData.guardianPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field full-width">
                        <label>ที่อยู่ที่ติดต่อได้</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.guardianAddress }}</span>
                        <input v-if="isEditing" v-model="formData.guardianAddress" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>สถานภาพของบิดามารดา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.pStatus }}</span>
                        <input v-if="isEditing" v-model="formData.pStatus" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>นักเรียนพักอาศัยอยู่กับใคร</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.liveWith }}</span>
                        <input v-if="isEditing" v-model="formData.liveWith" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ครอบครัวนักเรียนมีสมาชิกทั้งหมดกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.familyCount }}</span>
                        <input v-if="isEditing" v-model="formData.familyCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ปัจจุบันมีพี่น้องกำลังศึกษาอยู่กี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.sibStudy }}</span>
                        <input v-if="isEditing" v-model="formData.sibStudy" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.empCount }}</span>
                        <input v-if="isEditing" v-model="formData.empCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ไม่ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.unempCount }}</span>
                        <input v-if="isEditing" v-model="formData.unempCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>
            </div>
        </div>
      </div>
    </div>
</template>
  
<script setup>
    import { useRouter } from 'vue-router'
    import { ref } from 'vue';
    const user = {
      profileImage:'/images/User.png', 
      name: "นาย xxx xxx",
      nickName: "xxx",
      studentId: "xxxxxxxxx-x",
      studentPhone: "0903124512",
      studentClass: "ชั้นม.5/1", 
      studentBirthDate: "10 กันยายน 2550"}

    const formData = ref({
      address: "112 หมู่ 7 ต.หนองกอมเกาะ อ.เมืองหนองคาย จ.หนองคาย 43000",
      distance: "10 กิโลเมตร ",
      transport: "รถจักรยานยนต์",
      skills: "เต้น",
      fatherName: "นาย xxx xxx",
      fatherJob: "พนักงานบริษัท",
      fatherPhone: "0987654321",
      fatherSalary: "18000",
      fatherEducation: "ปริญญาตรี",
      motherName: "นาง xxx xxx",
      motherJob: "ข้าราชการ",
      motherPhone: "0978945613",
      motherSalary: "14000",
      motherEducation: "ปริญญาตรี",
      guardianName: "นาง xxx xxx",
      relation: "มารดา",
      guardianPhone: "0978945613",
      guardianAddress: "112 หมู่ 7  ต.หนองกอมเกาะ อ.เมืองหนองคาย จ.หนองคาย 43000",
      pStatus: "อยู่ด้วยกัน",
      liveWith: "บิดา-มารดา",
      familyCount: "4 คน",
      sibStudy: "1 คน",
      empCount: "2 คน",
      unempCount: "-"
    });
    
    const isEditing = ref(false);
    const toggleEdit = () => {
      if (isEditing.value) {
        // เมื่อบันทึกให้ส่งข้อมูลไปยังฐานข้อมูล
        console.log("Saving data:", formData.value);
        // ฟังก์ชันนี้สามารถเชื่อมต่อกับ API เพื่อบันทึกข้อมูลในอนาคตได้
      }
      isEditing.value = !isEditing.value;
    };


    const router = useRouter()
    const logout = () => {
    console.log("Logging out...");
    router.push('/')
    }

    const activeButton = ref('info');
    function setActive(button) {
        activeButton.value = button;
    }

    const isActive = ref('info'); 

const goToMap = () => {
  isActive.value = 'map'; // เปลี่ยนสถานะเป็นแผนที่
  router.push('/student/map-student'); // เปลี่ยนไปที่หน้า map-student
};

const goToProfile = () => {
  isActive.value = 'info'; // เปลี่ยนสถานะเป็นข้อมูล
  router.push('/student/profile-student'); // เปลี่ยนไปที่หน้า profile-student
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
  /* ปุ่มบันทึก */
  .save-button {
    background-color: #56A7F5; /* สีพื้นฐานของปุ่มบันทึก */
    color: white;
  }

  /* สีเมื่อ hover ปุ่มบันทึก */
  .save-button:hover {
    background-color: #62acf2; /* สีเมื่อ hover */
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

  .student-details {
    padding: 40px;
  }

  .row {
    display: flex;
    margin-bottom: 5px;
    justify-content: space-between; /* กระจาย item ให้มีพื้นที่เท่ากัน */
  }

  .field {
     flex: 1;
     margin: 5px;
  }
    
  .full-width {
    flex: 2;
  }

  label {
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px;
    margin-bottom: 5px;
    display: block;
    color: #5B5B5B;
  }

  .detail-span {
    background-color: #E1E1E1; /* สีพื้นหลัง */
    color: #5f5f5f; /* สีตัวหนังสือ */
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px; /* ขนาดตัวหนังสือ */
    padding: 8px; /* เพิ่ม padding */
    border-radius: 4px; /* มุมมน */
    display: block;
}
.Editdetail-span {
  width: 100%; /* ทำให้ input มีขนาดเท่ากับ container */
  padding: 5px; /* กำหนด padding เพื่อให้ input ดูเหมาะสม */
  background-color: #F9F9F9; /* สีพื้นหลัง */
  color: #3c3c3c; /* สีตัวหนังสือ */
  font-family: 'Inter', sans-serif;
  font-weight: 400;
  font-size: 14px; /* ขนาดตัวหนังสือ */
  padding: 8px; /* เพิ่ม padding */
  border-radius: 4px; /* มุมมน */
  border: 1px solid #3c3c3c; /* กำหนดเส้นขอบของ input */
  border-radius: 4px; /* มุมมน */
  box-sizing: border-box; /* ทำให้ padding ไม่ทำให้ขนาดของ input ใหญ่ขึ้น */
}
.Editdetail-span, .detail-span {
  transition: all 0.3s ease-in-out; /* เพิ่มการเปลี่ยนแปลงที่นุ่มนวล */
}
</style>