<template>
  <div class="image-upload-container">
    <h2 class="title">อัปโหลดรูปภาพ</h2>
    <p class="subtitle">เพิ่มเอกสารของคุณที่นี่ และคุณสามารถอัปโหลดไฟล์ได้สูงสุด 1 ไฟล์</p>
    <form @submit.prevent="uploadImage" class="upload-form">
      <!-- Drop Zone -->
      <div class="drop-zone" @dragover.prevent @drop.prevent="handleDrop" :class="{ 'is-dragover': isDragOver }"
        @dragenter.prevent="isDragOver = true" @dragleave.prevent="isDragOver = false" @click="triggerFileInput">
        <p v-if="!imageFile">ลากและวางไฟล์ที่นี่ หรือคลิกเพื่อเลือกไฟล์</p>
        <input type="file" @change="onFileChange" ref="fileInput" class="file-input" />
      </div>
      <!-- <button type="submit" class="upload-button">อัปโหลด</button> -->
    </form>

    <!-- แสดงพรีวิวภาพถ้ามีการเลือกไฟล์แล้ว พร้อมปุ่มลบ -->
    <div v-if="imagePreview" class="preview-container">
      <h3>พรีวิวภาพ</h3>
      <div class="image-preview-container">
        <img :src="imagePreview" alt="Image Preview" class="image-preview" />
        <button @click="clearImage" class="delete-button">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
            class="feather feather-x">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>
    </div>

    <p v-if="message" class="status-message">{{ message }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageFile: null,
      stuId: '',
      message: '',
      imagePreview: null, // เก็บ URL ของภาพพรีวิว
      isDragOver: false, // ใช้ตรวจจับสถานะการลากไฟล์
    };
  },
  methods: {
    triggerFileInput() {
      this.$refs.fileInput.click();
    },
    onFileChange(event) {
      const file = event.target.files[0];
      this.handleFile(file);
    },
    handleDrop(event) {
      this.isDragOver = false;
      const file = event.dataTransfer.files[0];
      this.handleFile(file);
    },
    handleFile(file) {
      this.imageFile = file;
      if (file) {
        this.imagePreview = URL.createObjectURL(file);
        this.$emit('file-selected', file); // Emit file to parent

      }
    },
    clearImage() {
      this.imageFile = null;
      this.imagePreview = null;
      this.$refs.fileInput.value = ''; // รีเซ็ต input file
    },
    async uploadImage() {
      if (!this.imageFile || !this.stuId) {
        this.message = 'อัปโหลดเสร็จสิ้น';
        return;
      }

      const formData = new FormData();
      formData.append('image', this.imageFile);
      formData.append('stu_id', this.stuId);

      try {
        const response = await axios.post('http://26.250.208.152:8081/api/images/', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        });
        this.message = 'อัปโหลดเสร็จสิ้น'; // ข้อความสำเร็จ
        this.clearImage(); // ล้างพรีวิวเมื่ออัปโหลดสำเร็จ
      } catch (error) {
        this.message = error.response?.data?.error || 'ไม่สามารถอัปโหลดภาพได้';

      }
    }
  },
};
</script>

<style scoped>
.image-upload-container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1.5rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fafafa;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 1rem;
  color: #333;
  margin-bottom: 0;
  text-align: left;
}

.subtitle {
  font-size: 0.875rem;
  color: #777;
  margin-bottom: 1rem;
  text-align: left;
}

.upload-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

/* Drop Zone */
.drop-zone {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  height: 150px;
  border: 2px dashed #ddd;
  border-radius: 8px;
  cursor: pointer;
  transition: border-color 0.3s ease;
  text-align: center;
  color: #999;
}

.drop-zone.is-dragover {
  border-color: #4CAF50;
  background-color: #f1f8e9;
}

.file-input {
  display: none;
  /* ซ่อน input file */
}

.text-input {
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  text-align: center;
}

.upload-button {
  padding: 0.5rem 1rem;
  border: none;
  background-color: #4CAF50;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease;
}

.upload-button:hover {
  background-color: #45a049;
}

.preview-container {
  margin-top: 1rem;
}

h3 {
  font-size: 1rem;
  margin-bottom: 0.5rem;
}

.image-preview-container {
  position: relative;
  display: block;
  width: 100%;
  /* ขยายความกว้างให้เต็ม */
}

.image-preview {
  width: 100%;
  /* กำหนดความกว้างให้เต็มที่ container */
  max-height: 300px;
  border-radius: 8px;
  margin-top: 0.5rem;
}

.delete-button {
  position: absolute;
  top: 15px;
  /* เพิ่ม margin-top ลงมา */
  right: 10px;
  background: none;
  /* เอาพื้นหลังออก */
  border: none;
  color: #ff4d4d;
  /* สีแดง */
  cursor: pointer;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.delete-button:hover {
  color: #e60000;
  /* เปลี่ยนสีแดงเข้มเมื่อ hover */
}

.status-message {
  margin-top: 1rem;
  color: green;
  font-weight: bold;
}
</style>
