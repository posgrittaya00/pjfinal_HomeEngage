<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div v-if="isLoading">
    ...loading
  </div>
  <div class="student-details" v-else>
    <div class="row">
      <div class="field">
        <label>ชื่อ-นามสกุล</label>
        <input v-model="formData.Name" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ชื่อเล่น</label>
        <input v-model="formData.Nickname" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>รหัสประจำตัว</label>
        <input v-model="formData.StuId" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เบอร์โทรศัพท์</label>
        <input v-model="formData.StuPhone" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ระดับชั้น</label>
        <input v-model="formData.StuClass" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
      <div class="field">
        <label>เกิดวันที่</label>
        <input v-model="formData.StuBirthDate" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field full-width">
        <label>ที่อยู่</label>
        <input v-model="formData.Address" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ระยะทางจากบ้านมาโรงเรียน</label>
        <input v-model="formData.Distance" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เดินทางมาโรงเรียนโดย</label>
        <input v-model="formData.Transport" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ความสามารถพิเศษ</label>
        <input v-model="formData.Skills" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
      <div class="field">
        <label>ชื่อ-นามสกุล (บิดา)</label>
        <input v-model="formData.FatherName" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>อาชีพ</label>
        <input v-model="formData.FatherJob" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เบอร์โทรศัพท์</label>
        <input v-model="formData.FatherPhone" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เงินเดือน</label>
        <input v-model="formData.FatherSalary" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ระดับการศึกษา</label>
        <input v-model="formData.FatherEdu" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
      <div class="field">
        <label>ชื่อ-นามสกุล (มารดา)</label>
        <input v-model="formData.MotherName" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>อาชีพ</label>
        <input v-model="formData.MotherJob" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เบอร์โทรศัพท์</label>
        <input v-model="formData.MotherPhone" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เงินเดือน</label>
        <input v-model="formData.MotherSalary" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ระดับการศึกษา</label>
        <input v-model="formData.MotherEdu" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
      <div class="field">
        <label>ชื่อ-นามสกุล (ผู้ปกครอง)</label>
        <input v-model="formData.ParentName" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เกี่ยวข้องเป็น</label>
        <input v-model="formData.Relation" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>เบอร์โทรศัพท์</label>
        <input v-model="formData.ParentPhone" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field full-width">
        <label>ที่อยู่ที่ติดต่อได้</label>
        <input v-model="formData.ParentAddress" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>สถานภาพของบิดามารดา</label>
        <input v-model="formData.PStatus" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
      <div class="field">
        <label>นักเรียนพักอาศัยอยู่กับใคร</label>
        <input v-model="formData.LivesWith" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ครอบครัวนักเรียนมีสมาชิกทั้งหมดกี่คน</label>
        <input v-model="formData.FamCount" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ปัจจุบันมีพี่น้องกำลังศึกษาอยู่กี่คน</label>
        <input v-model="formData.SibStudy" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ประกอบอาชีพกี่คน</label>
        <input v-model="formData.EmpCount" class="Editdetail-span" @blur="stopEditing" />
      </div>
      <div class="field">
        <label>ไม่ประกอบอาชีพกี่คน</label>
        <input v-model="formData.UnempCount" class="Editdetail-span" @blur="stopEditing" />
      </div>
    </div>

    <div class="row">
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  emits: ['updateData'],
  setup(props, { emit }) {
    const formData = ref({
      Name: "",
      Nickname: "",
      StuId: "",
      StuPhone: "",
      StuClass: "",
      StuBirthDate: "",
      Address: "",
      Distance: "",
      Transport: "",
      Skills: "",
      FatherName: "",
      FatherJob: "",
      FatherPhone: "",
      FatherSalary: "",
      FatherEdu: "",
      MotherName: "",
      MotherJob: "",
      MotherPhone: "",
      MotherSalary: "",
      MotherEdu: "",
      ParentName: "",
      Relation: "",
      ParentPhone: "",
      ParentAddress: "",
      PStatus: "",
      LivesWith: "",
      FamCount: "",
      SibStudy: "",
      EmpCount: "",
      UnempCount: ""
    });

    const isLoading = ref(true);
    const hasError = ref(false);
    const errorMessage = ref('');

    onMounted(async () => {
      try {
        const username = localStorage.getItem('username');
        let params = (username && props.StuId) ? props.StuId : username;
        if (username) {
          const response = await axios.get(`http://26.250.208.152:8000/api/student/${params}`);
          if (response && response.data) {
            formData.value = response.data;
            await emit('updateData', response.data.Name);
          } else {
            throw new Error("ข้อมูลนักเรียนไม่ถูกต้อง");
          }
        } else {
          throw new Error("ไม่พบชื่อผู้ใช้ใน localStorage");
        }

      } catch (error) {
        console.error("เกิดข้อผิดพลาดในการดึงข้อมูลนักเรียน:", error.message);
        hasError.value = true;
        errorMessage.value = error.message || "เกิดข้อผิดพลาดระหว่างการดึงข้อมูลนักเรียน";
      } finally {
        isLoading.value = false;
      }
    });

    return {
      formData,
      isLoading,
      hasError,
      errorMessage,
    };
  }
};
</script>

<style scoped>
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

.Editdetail-span {
    width: 100%; /* ทำให้ input มีขนาดเท่ากับ container */
    padding: 5px; /* กำหนด padding เพื่อให้ input ดูเหมาะสม */
    background-color: #F9F9F9; /* สีพื้นหลัง */
    color: #3c3c3c; /* สีตัวหนังสือ */
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px; /* ขนาดตัวหนังสือ */
    border: 1px solid #3c3c3c; /* กำหนดเส้นขอบของ input */
    border-radius: 4px; /* มุมมน */
    box-sizing: border-box; /* ทำให้ padding ไม่ทำให้ขนาดของ input ใหญ่ขึ้น */
}

.Editdetail-span {
    transition: all 0.3s ease-in-out; /* เพิ่มการเปลี่ยนแปลงที่นุ่มนวล */
}
</style>
