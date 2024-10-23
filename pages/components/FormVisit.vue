<template>
  <div>
    <h2 v-if="localSections && localSections.Name">{{ localSections.Name }}</h2>
    <h2 v-else>Loading...</h2> 
    <div v-for="section in localSections.Sections" :key="section.id" class="section" v-if="localSections && localSections.Sections">
      <h3>{{ section.Title }}</h3>
      <div v-for="field in section.Fields" :key="field.id" class="field">
        <label>{{ field.Label }}</label>
        <input v-if="field.fieldType === 'text'" type="text" v-model="field.value" />
        <input v-if="field.fieldType === 'radio'" type="radio" :name="field.label" v-model="field.value" />
        
      </div>
    </div>
  </div>
</template>
<script>
import { ref, watch, onMounted } from 'vue';
export default {
  props: {
    sections: {
      type: Object,
      default: null,
    },
  },
  setup(props) {
    const localSections = ref(props.sections);
    watch(() => props.sections, (newSections) => {
      if (newSections) {
        localSections.value = newSections;
        console.log("Updated localSections:", localSections.value);
      }
    });
    onMounted(() => {
      console.log("FormVisit received sections on mount:", localSections.value);
      if (localSections.value && localSections.value.Name) {
        console.log("Name field:", localSections.value.Name);
      }
    });
    return {
      localSections,
    };
  },
};
</script>