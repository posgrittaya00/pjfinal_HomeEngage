<template>
  <div>
    <h2 v-if="localSections && localSections.Name" class="main-title">{{ localSections.Name }}</h2>
    <h2 v-else class="main-title">Loading...</h2>

    <div v-if="localSections && localSections.Sections">
      <div v-for="section in localSections.Sections" :key="section.ID" class="section">
        <h3 class="section-title">{{ section.Title }}</h3>
        <div v-for="field in section.Fields" :key="field.ID" class="field">
          <div class="field-label">{{ field.Label }}</div>
          <div class="field-options">
            <div v-if="field.FieldType === 'radio'" class="radio-group">
              <div v-for="(option, index) in parseOptions(field.Options)" :key="index" class="radio-option">
                <input 
                  type="radio" 
                  :name="field.ID" 
                  :value="option" 
                  v-model="field.value" 
                  class="radio-input"
                />
                <label class="radio-label">{{ option }}</label>
              </div>
            </div>
            <input v-if="field.FieldType === 'textarea'" v-model="field.value" class="input-text" />
            <input v-if="field.FieldType === 'text'" type="text" v-model="field.value" class="input-text" />
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <p>No sections available</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
  sections: {
    type: Object,
    default: null,
  },
});

const localSections = ref(props.sections);

const parseOptions = (options) => {
  try {
    return JSON.parse(options);
  } catch (error) {
    console.error("Failed to parse options:", error);
    return [];
  }
};

watch(() => props.sections, (newSections) => {
  if (newSections) {
    localSections.value = newSections;
  }
});

onMounted(() => {
  console.log("FormVisit received sections on mount:", localSections.value);
});
</script>

<style scoped>
.main-title {
  font-size: 20px; /* ขนาดฟอนต์ */
  margin-bottom: 20px;
  max-width: 600px; /* กำหนดความกว้างสูงสุด */
  width: fit-content; /* ใช้ขนาดเนื้อหา */
  margin-left: 15px; /* ขยับไปทางซ้าย 20px */
}

.section {
  margin-bottom: 15px;
  margin-left: 15px; /* ขยับไปทางซ้าย 20px */
}

.section-title {
  background-color: #e0eefb; /* พื้นหลังสีฟ้าน้ำเงินอ่อน */
  color: #56A7F5; /* สีข้อความ */
  padding: 5px 10px; /* เพิ่มระยะห่างภายใน */
  border-radius: 5px; /* ทำมุมให้มน */
  font-size: 16px; /* ขนาดฟอนต์ */
  display: inline-block; /* ให้เป็นบล็อกในแนวนอน */
  margin-bottom: 5px; /* เพิ่มระยะห่างด้านล่าง */
  margin-left: 15px; /* ขยับไปทางซ้าย 20px */
}

.field {
  margin: 10px;
  margin-left: 15px; /* ขยับไปทางซ้าย 20px */
}

.field-label {
  font-size: 16px; /* ขนาดฟอนต์ */
  display: inline-block; /* ให้เป็นบล็อกในแนวนอน */
  margin-bottom: 10px; /* เพิ่มระยะห่างด้านล่าง */
  margin-left: 10px; /* ขยับไปทางซ้าย 20px */
}

.field-options {
  display: flex;
  flex-wrap: wrap; /* ให้ตัวเลือกสามารถลงไปได้เมื่อพื้นที่ไม่พอ */
  margin-left: 15px; /* ขยับไปทางซ้าย 20px */
}

.input-text {
  width: 50%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.radio-group {
  display: flex;
  flex-wrap: wrap; /* ทำให้ radio options เรียงตามแนวนอน */
}

.radio-option {
  display: flex; /* ใช้ flex เพื่อจัดตำแหน่ง */
  align-items: center; /* จัดตำแหน่งให้อยู่กลางแนวตั้ง */
  margin-right: 15px; /* เพิ่มระยะห่างระหว่าง radio options */
  margin-bottom: 12px; /* เพิ่มระยะห่างระหว่าง radio options แต่ละตัว */
}

.radio-input {
  margin-right: 5px; /* เพิ่มระยะห่างระหว่างปุ่ม radio กับข้อความ */
  width: 24px; /* ขยายขนาด radio button */
  height: 24px; /* ขยายขนาด radio button */
  appearance: none; /* ลบรูปแบบเริ่มต้นของ radio button */
  background-color: transparent; /* ไม่มีพื้นหลัง */
  border: 1px solid #ccc; /* ขอบเป็นสีเทาเมื่อไม่ถูกเลือก */
  border-radius: 50%; /* ทำให้เป็นวงกลม */
  outline: none; /* ลบกรอบเมื่อมีการเลือก */
  cursor: pointer; /* แสดงว่าปุ่มคลิกได้ */
  position: relative; /* เพื่อจัดการกับ pseudo-element */
}

/* เมื่อปุ่ม radio ถูกเลือก */
.radio-input:checked {
  background-color: white; /* พื้นหลังยังคงเป็นสีขาวเมื่อถูกเลือก */
  border: 1px solid #56A7F5; /* ขอบเป็นสีฟ้าเมื่อถูกเลือก */
}

/* วงกลมตรงกลางของปุ่มเมื่อถูกเลือก */
.radio-input:checked::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 12px; /* ขนาดวงกลมภายใน */
  height: 12px;
  background-color: #56A7F5; /* สีฟ้าตรงกลางเมื่อถูกเลือก */
  border-radius: 50%;
  transform: translate(-50%, -50%);
}

.radio-label {
  font-size: 14px; /* ขนาดฟอนต์สำหรับ label */
  line-height: 20px; /* ปรับความสูงของบรรทัดให้เหมาะสม */
}
</style>
