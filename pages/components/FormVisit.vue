<template>
    <div>
      <h2>ส่วนของฟอร์ม</h2>
      <div v-for="section in sections" :key="section.id" class="section">
        <h3>{{ section.title }}</h3>
        <div v-for="field in section.fields" :key="field.id" class="field">
          <label>{{ field.label }}</label>
          <input v-if="field.fieldType === 'text'" type="text" v-model="field.value" />
          <input v-if="field.fieldType === 'radio'" type="radio" :name="field.label" v-model="field.value" />
          <!-- จัดการประเภทของฟิลด์อื่นๆ ตามที่ต้องการ -->
        </div>
      </div>
    </div>
</template>

<script setup>
const props = defineProps({
  sections: {
    type: Array,
    default: () => []
  }
});
</script>