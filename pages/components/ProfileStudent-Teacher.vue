<template>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>ลำดับ</th>
          <th>รหัสประจำตัว</th>
          <th>ชื่อ-นามสกุล</th>
          <th>ระดับชั้น</th>
          <th>เทอมที่ 1</th>
          <th>เทอมที่ 2</th>
          <th>ข้อมูลนักเรียน</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(student, index) in students" :key="index">
          <td>{{ (index + 1) + ((currentPage - 1) * studentsPerPage) }}</td>
          <td>{{ student.StuId }}</td>
          <td>{{ student.Name }}</td>
          <td>{{ student.StuClass }}</td>
          <td>
            <button class="status-button" 
                    :class="{ visited: student.firstVisit, 'not-visited': !student.firstVisit }"
                    @click="goToFormStudent(student.stuId)">
              {{ student.firstVisit ? 'เยี่ยมแล้ว' : 'ยังไม่ได้เยี่ยม' }}
            </button>
          </td>
          <td>
            <button class="status-button" 
                    :class="{ visited: student.secondVisit, 'not-visited': !student.secondVisit }"
                    @click="goToFormStudent(student.stuId)">
              {{ student.secondVisit ? 'เยี่ยมแล้ว' : 'ยังไม่ได้เยี่ยม' }}
            </button>
          </td>
          <td>
            <button @click="goToInfoStudents" class="info-button">ข้อมูลนักเรียน</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { useRouter } from 'vue-router';

export default {
  props: {
    students: {
      type: Array,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
    studentsPerPage: {
      type: Number,
      required: true,
    },
  },
  setup() {
    const router = useRouter();

    const goToFormStudent = (stuId) => {
      // นำทางไปยังหน้า Form-Student และส่ง studentId
      router.push(`/teacher/form-student/`);
    };

    const goToInfoStudents = () => {
      router.push('/teacher/Info-Students'); // นำทางไปยังเส้นทางที่ต้องการ
    };

    return {
      goToInfoStudents,
      goToFormStudent,
    };
  },
};
</script>

<style scoped>
.table-container {
  max-width: 100%;
  overflow-x: auto;
  margin: 0 80px;
  display: flex;
  justify-content: center;
}

table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  font-family: 'Inter', sans-serif;
  margin: auto;
}

thead {
  background-color: #f9f9f9;
  border-bottom: 2px solid #ddd;
}

th, td {
  padding: 8px 12px;
  font-weight: 400;
  border-bottom: 1px solid #ddd;
  vertical-align: middle;
  text-align: center;
}

th {
  font-size: 16px;
  color: #969696;
  white-space: nowrap;
}

td {
  font-size: 16px;
  color: #5B5B5B;
}

.status-button, .info-button {
  padding: 6px 10px;
  border: none;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  width: 90%;
  height: 38px;
}

.status-button {
  color: white;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.visited {
  background-color: #76BC43; /* สีเขียว */
}

.not-visited {
  background-color: #EF5757; /* สีแดง */
}

.info-button {
  background-color: #79BEFF; /* สีฟ้า */
  color: white;
}

.info-button:hover {
  background-color: #80c2ff;
}

tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

.pagination-wrapper {
  margin-top: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
}

.pagination-wrapper button {
  margin: 0 6px;
}
</style>
