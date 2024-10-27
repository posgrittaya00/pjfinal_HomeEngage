<template>
  <div v-if="localSections" v-for="(context, contextIndex) in localSections" :key="contextIndex" class="section">
    <h3 class="main-section">{{ context.Name }}</h3>
    <div v-for="(section, sectionIndex) in context.Sections" :key="sectionIndex" class="section">
      <h3 class="section-title">{{ section.Title }}</h3>
      <div v-for="(field, fieldIndex) in section.Fields" :key="field.ID" class="field" :id="`field-${field.ID}`">
        <div class="label-section">
          <div class="field-label" v-if="!section.HasOptionsOnly">
            {{ `${contextIndex + 1}.${fieldIndex + 1}` }} {{ field.Label }}
          </div>

          <div class="rating">
            <label v-for="point in 5" :key="point"
              :class="['rating-label', { 'selected': ratings[field.ID] === point }]">
              <input type="radio" :name="`rating-${field.ID}`" :value="point" v-model="ratings[field.ID]"
                class="rating-input" />
              <span class="rating-number">{{ point }}</span>
            </label>
          </div>
        </div>

        <div class="field-options">
          <div v-if="field.FieldType === 'radio'" class="radio-group">
            <div v-for="(option, index) in parseOptions(field.Options)" :key="index" class="radio-option">
              <label class="radio-label">
                <input type="radio" :name="field.ID" :value="option" class="radio-input" v-model="field.value" />
                {{ option }}
              </label>
            </div>
          </div>
          <input v-if="field.FieldType === 'textarea'" v-model="field.value" class="input-text" />
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <p>No sections available</p>
  </div>
  <div class="input-group">
    <label>ลงชื่อผู้ปกครองนักเรียน</label>
    <input type="text" v-model="names" class="input-text" />
  </div>
  <ImageForm/>
  <div class="button-container">
    <button @click="saveForm" class="save-button">บันทึก</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import ImageForm from './ImageForm.vue';

const names = ref('');

const props = defineProps({
  sections: {
    type: Array,
    default: null,
  },
  studentID: {
    type: String,
    default: null
  },
  term: {
    type: String,
    default: null
  }
});

const localSections = ref(props.sections);

// Initialize ratings object to store ratings by field ID
const ratings = ref({});

const parseOptions = (options) => {
  try {
    const sanitizedOptions = options.replace(/\n/g, '').trim();
    return JSON.parse(sanitizedOptions);
  } catch (error) {
    console.error("Failed to parse options:", error);
    return [];
  }
};

// Include ratings in the form data when saving
const saveForm = async () => {
  let isFormComplete = true;
  let firstIncompleteField = null;

  // ตรวจสอบว่า radio, textarea, และ rating กรอกครบหรือไม่
  localSections.value.forEach((context) => {
    context.Sections.forEach((section) => {
      section.Fields.forEach((field) => {
        if (field.FieldType === 'radio' && (!field.value || field.value === "")) {
          isFormComplete = false;
          if (!firstIncompleteField) firstIncompleteField = `field-${field.ID}`;
        }
        if (field.FieldType === 'textarea' && (!field.value || field.value.trim() === "")) {
          isFormComplete = false;
          if (!firstIncompleteField) firstIncompleteField = `field-${field.ID}`;
        }
        if (field.FieldType === 'rating' && (!ratings.value[field.ID] || ratings.value[field.ID] === null)) {
          isFormComplete = false;
          if (!firstIncompleteField) firstIncompleteField = `field-${field.ID}`;
        }
      });
    });
  });

  // ถ้าข้อมูลไม่ครบ แสดงการแจ้งเตือนและเลื่อนไปที่ฟิลด์หลังจากกดโอเค
  if (!isFormComplete) {
    alert("กรุณากรอกข้อมูลให้ครบถ้วนในช่องที่จำเป็น");
    setTimeout(() => {
      const element = document.getElementById(firstIncompleteField);
      if (element) {
        element.scrollIntoView({ behavior: "smooth", block: "center" });
      } else {
        console.error(`ไม่พบฟิลด์ ${firstIncompleteField}`);
      }
    }, 0);
    return;
  }

  const teacherID = localStorage.getItem("username");
  const formData = {
    teacher_id: teacherID,
    student_id: props.studentID,
    term: props.term,
    sections: localSections.value.map(context => ({
      section_id: context.ID,
      title: context.Name,
      fields: context.Sections.flatMap(section => 
        section.Fields.map(field => ({
          field_id: field.ID,
          value: field.value || "",
          score: ratings.value[field.ID] || 1
        }))
      )
    }))
  };

  try {
    const response = await axios.post('http://localhost:8081/api/form-responses/create', formData);
    if (response.data) {
      alert('บันทึกฟอร์มสำเร็จ!');
      this.$router.push('/teacher/Home-Teacher');
    } else {
      alert('บันทึกฟอร์มไม่สำเร็จ');
    }
  } catch (error) {
    console.error('ไม่สามารถบันทึกฟอร์มได้:', error);
    alert('บันทึกฟอร์มไม่สำเร็จ');
  }
};
</script>

<style scoped>
.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
  margin-left: 60px;
}

.input-group label {
  font-size: 16px;
  margin-bottom: 5px;
}

.input-group .input-text {
  width: 49.6%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.rating {
  display: flex;
  gap: 10px;
}

.rating-number {
  font-weight: bold;
  font-size: 16px;
}

.rating-input {
  display: none;
  /* Hide the actual radio input */
}

.rating-label {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 18px;
  height: 18px;
  border: 2px solid #333;
  border-radius: 50%;
  text-align: center;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.rating-number {
  font-weight: bold;
  font-size: 18px;
}

.rating-input {
  display: none;
}

.rating-label:hover {
  background-color: #ddddf3;
  color: #333;
}

.selected {
  background-color: #333;
  color: white;
  border-color: #333;
}

.label-section {
  display: flex;
  justify-content: space-between;
  max-width: 50vw;
  width: 100%;
}

.main-section {
  background-color: #b695f3;
  color: #ffffff;
  padding: 5px 10px;
  border-radius: 3px;
  font-size: 18px;
  display: inline-block;
  margin-bottom: 5px;
  margin-left: 15px;
}

.main-title {
  font-size: 20px;
  margin-bottom: 20px;
  max-width: 600px;
  width: fit-content;
  margin-left: 15px;
}

.section {
  margin-bottom: 15px;
  margin-left: 15px;
}

.section-title {
  background-color: #e0eefb;
  color: #56A7F5;
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 16px;
  display: inline-block;
  margin-bottom: 5px;
  margin-left: 15px;
}

.field {
  margin: 10px;
  margin-left: 15px;
}

.field-label {
  font-size: 16px;
  display: inline-block;
  margin-bottom: 10px;
  margin-left: 10px;
}

.field-options {
  display: flex;
  flex-wrap: wrap;
  margin-left: 15px;
}

.input-text {
  width: 50%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.radio-group {
  display: flex;
  flex-wrap: wrap;
}

.radio-option {
  display: flex;
  align-items: center;
  margin-right: 15px;
  margin-bottom: 12px;
}

.radio-input {
  margin-right: 5px;
  width: 16px;
  height: 16px;
  appearance: none;
  background-color: transparent;
  border: 1px solid #ccc;
  border-radius: 50%;
  outline: none;
  cursor: pointer;
  position: relative;
}

.radio-input:checked {
  background-color: white;
  border: 1px solid #56A7F5;
}

.radio-input:checked::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 12px;
  height: 12px;
  background-color: #56A7F5;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}

.radio-label {
  --bordersize: 10px;
  cursor: pointer;
  font-size: 16px;
  padding: 0.25rem;
  display: flex;
  align-items: flex-start;
  border-radius: var(--bordersize);
  transition: background-color 0.3s ease;
}

.radio-label:hover {
  background-color: #ddddf3;
  border-radius: calc(var(--bordersize * 0.2));
}



.button-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.save-button {
  padding: 10px 20px;
  background-color: #56A7F5;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.save-button:hover {
  background-color: #4195e0;
}
</style>