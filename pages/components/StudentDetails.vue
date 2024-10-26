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
                        <span class="detail-span">{{ formData.name }}</span>
                    </div>
                    <div class="field">
                        <label>ชื่อเล่น</label>
                        <span class="detail-span">{{ formData.nickname }}</span>
                    </div>
                    <div class="field">
                        <label>รหัสประจำตัว</label>
                        <span class="detail-span">{{ formData.stu_id }}</span>
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span">{{ formData.stu_phone }}</span>
                    </div>
                    <div class="field">
                        <label>ระดับชั้น</label>
                        <span class="detail-span">{{ formData.stu_class }}</span>
                    </div>
                </div>

                <div class="row">
                  <div class="field">
                        <label>เกิดวันที่</label>
                        <span class="detail-span">{{ formData.stu_birth_date }}</span>
                    </div>
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
                        <span class="detail-span" v-if="!isEditing">{{ formData.father_name }}</span>
                        <input v-if="isEditing" v-model="formData.father_name" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.father_job }}</span>
                        <input v-if="isEditing" v-model="formData.father_job" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.father_phone }}</span>
                        <input v-if="isEditing" v-model="formData.father_phone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.father_salary }}</span>
                        <input v-if="isEditing" v-model="formData.father_salary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.father_edu }}</span>
                        <input v-if="isEditing" v-model="formData.father_edu" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (มารดา)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.mother_name }}</span>
                        <input v-if="isEditing" v-model="formData.mother_name" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.mother_job }}</span>
                        <input v-if="isEditing" v-model="formData.mother_job" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.mother_phone }}</span>
                        <input v-if="isEditing" v-model="formData.mother_phone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.mother_salary }}</span>
                        <input v-if="isEditing" v-model="formData.mother_salary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.mother_edu }}</span>
                        <input v-if="isEditing" v-model="formData.mother_edu" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (ผู้ปกครอง)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.parent_name }}</span>
                        <input v-if="isEditing" v-model="formData.parent_name" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เกี่ยวข้องเป็น</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.relation }}</span>
                        <input v-if="isEditing" v-model="formData.relation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.parent_phone }}</span>
                        <input v-if="isEditing" v-model="formData.parent_phone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field full-width">
                        <label>ที่อยู่ที่ติดต่อได้</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.parent_address }}</span>
                        <input v-if="isEditing" v-model="formData.parent_address" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>สถานภาพของบิดามารดา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.p_status }}</span>
                        <input v-if="isEditing" v-model="formData.p_status" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>นักเรียนพักอาศัยอยู่กับใคร</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.lives_with }}</span>
                        <input v-if="isEditing" v-model="formData.lives_with" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ครอบครัวนักเรียนมีสมาชิกทั้งหมดกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.fam_count }}</span>
                        <input v-if="isEditing" v-model="formData.fam_count" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ปัจจุบันมีพี่น้องกำลังศึกษาอยู่กี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.sib_study }}</span>
                        <input v-if="isEditing" v-model="formData.sib_study" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.emp_count }}</span>
                        <input v-if="isEditing" v-model="formData.emp_count" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ไม่ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.unemp_count }}</span>
                        <input v-if="isEditing" v-model="formData.unemp_count" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  props: {
    isEditing: {
      type: Boolean,
      required: true,
    },
    onToggleEdit: {
      type: Function,
      required: true,
    },
    StuId:{
      type: String,
    }
  },
  emits: ['updateData'],  // Correctly declare the emitted event
  setup(props, { emit }) {
    const formData = ref({
      name: "",
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
        console.log(username, props.StuId)

        let params = (username && props.StuId) ? props.StuId : username
        if (username) {
          const response = await axios.get(`http://26.250.208.152:8000/api/student/${params}`);
          if (response && response.data) {
            formData.value = response.data;  // Assign the fetched data
            await emit('updateData', response.data.Name);  // Emit the data to parent
          } else {
            throw new Error("Invalid student data");
          }
        } else {
          throw new Error("No username found in localStorage");
        }

      } catch (error) {
        console.error("Error fetching student data:", error.message);
        hasError.value = true;
        errorMessage.value = error.message || "An error occurred while fetching student data.";
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