<template>
  <div class="sidebar">
    <div class="profile">
      <div class="avatar"></div>
      <div class="user-info">
        <p class="name">นาย xxx xxx</p>
        <p class="details">ชั้น ม.5/1 รหัสประจำตัว xxxxxxxxxx-x</p>
      </div>
    </div>
    <div class="menu">
      <nuxt-link to="/" class="menu-item">
        <i class="icon-home"></i>
        หน้าแรก
      </nuxt-link>
      <nuxt-link to="/profile" class="menu-item">
        <i class="icon-profile"></i>
        โปรไฟล์
      </nuxt-link>
      <nuxt-link to="/appointment" class="menu-item">
        <i class="icon-calendar"></i>
        จองวันเยี่ยมบ้าน
      </nuxt-link>
    </div>
    <div class="logout">
      <button @click="logout" class="logout-button">
        <i class="icon-logout"></i>
        Logout
      </button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    logout() {
      // เขียน logic การ logout ที่นี่
      console.log('User logged out');
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 260px;
  background-color: #ffffff;
  padding: 30px 20px;
  border-right: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1); /* เพิ่มเงาให้ sidebar */
  transition: all 0.3s ease-in-out;
}

.profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}

.avatar {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  background-color: #d8d8d8;
  margin-bottom: 15px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); /* เพิ่มเงาให้ avatar */
}

.user-info {
  text-align: center;
}

.name {
  font-weight: bold;
  margin-bottom: 5px;
  font-size: 1.1em;
  color: #333;
}

.details {
  font-size: 0.9em;
  color: #888;
}

.menu {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 12px 0;
  font-size: 1.1em;
  color: #333;
  text-decoration: none;
  border-radius: 5px;
  transition: background-color 0.2s ease;
}

.menu-item i {
  margin-right: 12px;
  font-size: 1.2em;
}

.menu-item:hover {
  background-color: #f5f5f5; /* เพิ่มสีเมื่อ hover */
  color: #1976d2; /* เปลี่ยนสีเมื่อ hover */
}

.logout {
  margin-top: 20px;
}

.logout-button {
  width: 100%;
  background-color: #ff6b6b;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 5px;
  font-size: 1.1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 4px 8px rgba(255, 107, 107, 0.3); /* เพิ่มเงาให้ปุ่ม logout */
  transition: background-color 0.2s ease, box-shadow 0.2s ease;
}

.logout-button i {
  margin-right: 10px;
  font-size: 1.2em;
}

.logout-button:hover {
  background-color: #ff5252;
  box-shadow: 0px 6px 12px rgba(255, 82, 82, 0.4); /* เพิ่มเงาเมื่อ hover */
}
</style>
