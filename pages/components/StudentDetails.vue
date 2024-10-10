<template>
    <Head>
      <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    </Head>
    <div class="student-details">
                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล</label>
                        <span class="detail-span">{{ formData.Name }}</span>
                    </div>
                    <div class="field">
                        <label>ชื่อเล่น</label>
                        <span class="detail-span">{{ formData.NickName }}</span>
                    </div>
                    <div class="field">
                        <label>รหัสประจำตัว</label>
                        <span class="detail-span">{{ formData.stuId }}</span>
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span">{{ formData.stuPhone }}</span>
                    </div>
                    <div class="field">
                        <label>ระดับชั้น</label>
                        <span class="detail-span">{{ formData.stuClass }}</span>
                    </div>
                    <div class="field">
                        <label>เกิดวันที่</label>
                        <span class="detail-span">{{ formData.stuBirthDate }}</span>
                    </div>
                </div>

                <div class="row">
                    <div class="field full-width">
                        <label>ที่อยู่</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Address }}</span>
                        <input v-if="isEditing" v-model="formData.Address" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระยะทางจากบ้านมาโรงเรียน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Distance }}</span>
                        <input v-if="isEditing" v-model="formData.Distance" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เดินทางมาโรงเรียนโดย</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Transport }}</span>
                        <input v-if="isEditing" v-model="formData.Transport" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ความสามารถพิเศษ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Skills }}</span>
                        <input v-if="isEditing" v-model="formData.Skills" class="Editdetail-span" @blur="stopEditing"  />
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
                        <span class="detail-span" v-if="!isEditing">{{ formData.fatherEdu }}</span>
                        <input v-if="isEditing" v-model="formData.fatherEdu" class="Editdetail-span" @blur="stopEditing"  />
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
                        <span class="detail-span" v-if="!isEditing">{{ formData.motherEdu }}</span>
                        <input v-if="isEditing" v-model="formData.motherEdu" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (ผู้ปกครอง)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentName }}</span>
                        <input v-if="isEditing" v-model="formData.ParentName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เกี่ยวข้องเป็น</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Relation }}</span>
                        <input v-if="isEditing" v-model="formData.Relation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentPhone }}</span>
                        <input v-if="isEditing" v-model="formData.ParentPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field full-width">
                        <label>ที่อยู่ที่ติดต่อได้</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentAddress }}</span>
                        <input v-if="isEditing" v-model="formData.ParentAddress" class="Editdetail-span" @blur="stopEditing"  />
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
                        <span class="detail-span" v-if="!isEditing">{{ formData.famCount }}</span>
                        <input v-if="isEditing" v-model="formData.famCount" class="Editdetail-span" @blur="stopEditing"  />
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
</template>

<script setup>
import { ref } from 'vue';

const formData = ref({
    Name: "นาย xxx xxx",
    NickName: "xxx",
    stuId: "xxxxxxxxx-x",
    stuPhone: "0903124512",
    stuClass: "ชั้นม.5/1", 
    stuBirthDate: "10 กันยายน 2550",
    Address: "112 หมู่ 7 ต.หนองกอมเกาะ อ.เมืองหนองคาย จ.หนองคาย 43000",
    Distance: "10 กิโลเมตร",
    Transport: "รถจักรยานยนต์",
    Skills: "เต้น",
    fatherName: "นาย xxx xxx",
    fatherJob: "พนักงานบริษัท",
    fatherPhone: "0987654321",
    fatherSalary: "18000",
    fatherEdu: "ปริญญาตรี",
    motherName: "นาง xxx xxx",
    motherJob: "ข้าราชการ",
    motherPhone: "0978945613",
    motherSalary: "14000",
    motherEdu: "ปริญญาตรี",
    ParentName: "นาง xxx xxx",
    Relation: "มารดา",
    ParentPhone: "0978945613",
    ParentAddress: "112 หมู่ 7 ต.หนองกอมเกาะ อ.เมืองหนองคาย 43000",
    pStatus: "อยู่ด้วยกัน",
    liveWith: "บิดา-มารดา",
    famCount: "4 คน",
    sibStudy: "1 คน",
    empCount: "2 คน",
    unempCount: "-"
});

defineProps({
  isEditing: Boolean,
  onToggleEdit: Function
});

// การเปลี่ยนค่า formData จะถูกจับด้วย watch เพื่อให้ทำงานได้ตามที่แก้ไข
watch(formData, (newVal) => {
  console.log("Form Data updated:", newVal);
});
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