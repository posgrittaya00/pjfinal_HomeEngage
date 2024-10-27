<template>
    <div class="image-upload-container">
      <h2>Upload Image</h2>
      <form @submit.prevent="uploadImage" class="upload-form">
        <input type="file" @change="onFileChange" class="file-input" />
        <input type="text" v-model="stuId" placeholder="Student ID" class="text-input" />
        <button type="submit" class="upload-button">Upload</button>
      </form>
  
      <!-- แสดงพรีวิวภาพถ้ามีการเลือกไฟล์แล้ว -->
      <div v-if="imagePreview" class="preview-container">
        <h3>Image Preview:</h3>
        <img :src="imagePreview" alt="Image Preview" class="image-preview" />
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
      };
    },
    methods: {
      onFileChange(event) {
        const file = event.target.files[0];
        this.imageFile = file;
  
        // ถ้ามีไฟล์ เลือกใช้ URL เพื่อสร้างพรีวิวภาพ
        if (file) {
          this.imagePreview = URL.createObjectURL(file);
        }
      },
      async uploadImage() {
        if (!this.imageFile || !this.stuId) {
          this.message = 'Please provide both image and student ID.';
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
          this.message = response.data.message;
          this.imagePreview = null; // ล้างพรีวิวเมื่ออัปโหลดสำเร็จ
        } catch (error) {
          this.message = error.response?.data?.error || 'Failed to upload image';
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .image-upload-container {
    max-width: 400px;
    margin: 2rem auto;
    padding: 1.5rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #fafafa;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  h2 {
    font-size: 1.8rem;
    color: #333;
    margin-bottom: 1rem;
  }
  
  .upload-form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  
  .file-input, .text-input {
    padding: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  .text-input {
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
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
  }
  
  .image-preview {
    max-width: 100%;
    max-height: 300px;
    border-radius: 8px;
    margin-top: 0.5rem;
  }
  
  .status-message {
    margin-top: 1rem;
    color: green;
    font-weight: bold;
  }
  </style>
  