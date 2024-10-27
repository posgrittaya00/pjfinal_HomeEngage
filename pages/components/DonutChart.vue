<template>
  <div>
    <div class="term-selector">
      <button @click="setTerm('term1')" :class="['term-button', { active: activeTerm === 'term1' }]">เทอม 1</button>
      <button @click="setTerm('term2')" :class="['term-button', { active: activeTerm === 'term2' }]">เทอม 2</button>
    </div>
    <div class="button-group">
      <button @click="setActiveChart('chart1')"
        :class="['button', 'chart1', { active: activeChart === 'chart1' }]">บริบท</button>
      <button @click="setActiveChart('chart2')"
        :class="['button', 'chart2', { active: activeChart === 'chart2' }]">ครอบครัว</button>
      <button @click="setActiveChart('chart3')"
        :class="['button', 'chart3', { active: activeChart === 'chart3' }]">นักเรียน</button>
      <button @click="setActiveChart('chart4')"
        :class="['button', 'chart4', { active: activeChart === 'chart4' }]">ความต้องการ</button>
    </div>

    <!-- แสดงข้อมูลในตารางและกราฟ -->
    <div v-show="activeChart === 'chart1'" id="chart1" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart1'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data1WithPercentage" :key="item.level">
          <td>{{ item.level }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.count }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <div v-show="activeChart === 'chart2'" id="chart2" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart2'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data2WithPercentage" :key="item.level">
          <td>{{ item.level }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.count }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <div v-show="activeChart === 'chart3'" id="chart3" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart3'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data3WithPercentage" :key="item.level">
          <td>{{ item.level }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.count }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <div v-show="activeChart === 'chart4'" id="chart4" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart4'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data4WithPercentage" :key="item.level">
          <td>{{ item.level }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.count }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';
import * as echarts from 'echarts';

const activeChart = ref('chart1');
const activeTerm = ref('term1');
const totalStudents = ref(0);
const dataWithPercentage = ref({ chart1: [], chart2: [], chart3: [], chart4: [] });

const fetchData = async () => {
  try {
    const response = await axios.get('http://26.250.208.152:8081/api/student/summary');
    const data = response.data;

    // กำหนดข้อมูลตามเทอมที่เลือก
    if (activeTerm.value === 'term1') {
      totalStudents.value = data.terms['1'].total_students;
      dataWithPercentage.value = {
        chart1: data.terms['1'].sections['บริบท ( Context – C )'],
        chart2: data.terms['1'].sections['ครอบครัว ( Family – F )'],
        chart3: data.terms['1'].sections['นักเรียน (Student – S )'],
        chart4: data.terms['1'].sections['ความต้องการต่อโรงเรียน / อบจ. ( School – S )']
      };
    } else {
      totalStudents.value = data.terms['2']?.total_students || 0;
      dataWithPercentage.value = {
        chart1: data.terms['2']?.sections['บริบท ( Context – C )'] || [],
        chart2: data.terms['2']?.sections['ครอบครัว ( Family – F )'] || [],
        chart3: data.terms['2']?.sections['นักเรียน (Student – S )'] || [],
        chart4: data.terms['2']?.sections['ความต้องการต่อโรงเรียน / อบจ. ( School – S )'] || []
      };
    }

    // อัปเดตกราฟที่กำลังแสดงอยู่
    setActiveChart(activeChart.value);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

// กำหนดข้อมูลสำหรับแต่ละกราฟ
const data1WithPercentage = computed(() =>
  dataWithPercentage.value.chart1.map(item => ({
    ...item,
    percentage: parseFloat(item.percentage).toFixed(2)
  }))
);

const data2WithPercentage = computed(() =>
  dataWithPercentage.value.chart2.map(item => ({
    ...item,
    percentage: parseFloat(item.percentage).toFixed(2)
  }))
);

const data3WithPercentage = computed(() =>
  dataWithPercentage.value.chart3.map(item => ({
    ...item,
    percentage: parseFloat(item.percentage).toFixed(2)
  }))
);

const data4WithPercentage = computed(() =>
  dataWithPercentage.value.chart4.map(item => ({
    ...item,
    percentage: parseFloat(item.percentage).toFixed(2)
  }))
);


const createChart = (chartId, title, colors, data) => {
  const myChart = echarts.init(document.getElementById(chartId));
  const option = {
    title: {
      text: title,
      left: 'center',
      top: 'center',
      textStyle: {
        fontSize: 16,
        color: '#666',
        fontFamily: 'Inter'
      }
    },
    tooltip: {
      trigger: 'item',
      formatter: '{a} <br/>{b}: {c} คน ({d}%)',
      textStyle: {
        fontFamily: 'Inter'
      }
    },
    legend: {
      orient: 'horizontal',
      bottom: 0,
      data: data.map(item => item.level),  // เปลี่ยนจาก item.name เป็น item.level
      itemHeight: 12,
      textStyle: {
        color: '#999',
        fontSize: 14,
        fontFamily: 'Inter'
      }
    },
    series: [
      {
        name: 'สถานะนักเรียน',
        type: 'pie',
        radius: ['40%', '70%'],
        avoidLabelOverlap: false,
        label: {
          show: true,
          position: 'inside',
          formatter: params => {
            const studentData = data.find(item => item.level === params.name);
            return studentData && studentData.count > 0
              ? `${parseFloat(studentData.percentage).toFixed(2)}%`
              : '';
          },
          textStyle: {
            fontSize: 14,
            color: '#fff',
            fontFamily: 'Inter'
          }
        },
        labelLine: { show: false },
        data: data.map(item => ({
          name: item.level, // ชื่อที่จะแสดงในกราฟ
          value: item.count, // จำนวนในกราฟ
          percentage: item.percentage // เพิ่ม percentage ไว้แสดง
        })),
        itemStyle: {
          color: function (params) {
            return colors[params.dataIndex];
          }
        }
      }
    ]
  };
  myChart.setOption(option);
};

// ฟังก์ชันตั้งค่าและอัปเดตกราฟตามแผนภูมิและเทอมที่เลือก
const setActiveChart = (chartId) => {
  activeChart.value = chartId;
  const data = dataWithPercentage.value[chartId];
  const titleMap = {
    chart1: 'สรุปบริบท',
    chart2: 'สรุปครอบครัว',
    chart3: 'สรุปนักเรียน',
    chart4: 'สรุปความต้องการ'
  };
  const colorsMap = {
    chart1: ['#C88CFF', '#D8A2FF', '#E6B6FF', '#F0C3FF', '#FBD0FF'],
    chart2: ['#6DB9FF', '#79C6FE', '#81D8FF', '#97E6FF', '#ADF2FF'],
    chart3: ['#FFA400', '#FFAE2A', '#FFAF3E', '#FFC27E', '#FFD7AA'],
    chart4: ['#FF4545', '#FF5757', '#FF8784', '#FFA09D', '#FFABAB']
  };

  setTimeout(() => {
    createChart(chartId, titleMap[chartId], colorsMap[chartId], data);
  }, 100);
};

// ฟังก์ชันเปลี่ยนเทอมและอัปเดตข้อมูล
const setTerm = (term) => {
  activeTerm.value = term;
  fetchData(); // ดึงข้อมูลใหม่เมื่อเปลี่ยนเทอม
};

// เรียกใช้ fetchData เมื่อคอมโพเนนต์ mount
onMounted(() => {
  fetchData();
});
</script>

<style scoped>
.button-group {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.term-selector {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

.term-button {
  background-color: #ECECEC;
  color: #56A7F5;
  border: none;
  padding: 10px 20px;
  margin: 0 5px;
  border-radius: 5px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  transition: background-color 0.3s, color 0.3s;
  min-width: 120px;
}

.term-button.active {
  background-color: #56A7F5;
  color: #ECECEC;
}

.term-button:hover {
  background-color: #D3D3D3;
  color: #56A7F5;
}

.button {
  background-color: #ECECEC;
  color: #5B5B5B;
  border: none;
  padding: 10px 20px;
  margin: 0 5px;
  border-radius: 5px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  transition: background-color 0.3s, color 0.3s;
  min-width: 120px;
}

.button.active {
  color: #ffffff;
}

.button.chart1.active {
  background-color: #F0C3FF;
}

.button.chart2.active {
  background-color: #97E6FF;
}

.button.chart3.active {
  background-color: #FFC27E;
}

.button.chart4.active {
  background-color: #FF8784;
}

.button:hover {
  background-color: #D3D3D3;
}

.button.chart1:hover {
  background-color: #F0C3FF;
  color: white;
}

.button.chart2:hover {
  background-color: #97E6FF;
  color: white;
}

.button.chart3:hover {
  background-color: #FFC27E;
  color: white;
}

.button.chart4:hover {
  background-color: #FF8784;
  color: white;
}

.chart {
  margin-top: 20px;
}

.summary {
  margin-top: 20px;
  font-family: 'Inter', sans-serif;
}

h3 {
  color: #333;
  text-align: left;
  margin-left: 360px;
  margin-bottom: 10px;
  font-size: 14px;
  font-family: 'Inter', sans-serif;
}

table {
  width: 40%;
  margin-left: auto;
  margin-right: auto;
  border-collapse: collapse;
  margin-top: 0;
  font-family: 'Inter', sans-serif;
}

table,
td {
  border: none;
  padding: 4px;
  text-align: left;
  font-size: 14px;
  color: #666;
}

tr:nth-child(odd) {
  background-color: #f9f9f9;
}
</style>
