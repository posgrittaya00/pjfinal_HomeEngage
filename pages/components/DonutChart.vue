<template>
  <div>
    <div class="button-group">
        <button @click="setActiveChart('chart1')" :class="['button', 'chart1', { active: activeChart === 'chart1' }]">บริบท</button>
        <button @click="setActiveChart('chart2')" :class="['button', 'chart2', { active: activeChart === 'chart2' }]">ครอบครัว</button>
        <button @click="setActiveChart('chart3')" :class="['button', 'chart3', { active: activeChart === 'chart3' }]">นักเรียน</button>
        <button @click="setActiveChart('chart4')" :class="['button', 'chart4', { active: activeChart === 'chart4' }]">ความต้องการ</button>
    </div>

    <!-- กราฟที่ 1 -->
    <div v-show="activeChart === 'chart1'" id="chart1" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart1'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data1WithPercentage" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.value }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <!-- กราฟที่ 2 -->
    <div v-show="activeChart === 'chart2'" id="chart2" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart2'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data2WithPercentage" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.value }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <!-- กราฟที่ 3 -->
    <div v-show="activeChart === 'chart3'" id="chart3" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart3'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data3WithPercentage" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.value }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>

    <!-- กราฟที่ 4 -->
    <div v-show="activeChart === 'chart4'" id="chart4" class="chart" style="width: 100%; height: 400px;"></div>
    <div v-show="activeChart === 'chart4'" class="summary">
      <h3>จำนวนนักเรียนทั้งหมด {{ totalStudents }} คน</h3>
      <table>
        <tr v-for="item in data4WithPercentage" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.value }} คน</td>
          <td>{{ item.percentage }}%</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';

const totalStudents = 21;
const activeChart = ref('chart1');
const setActiveChart = (chartId) => {
  activeChart.value = chartId;
  
  // เรียก resize เมื่อเปลี่ยนแผนภูมิ
  setTimeout(() => {
    if (chartId === 'chart1') {
      echarts.getInstanceByDom(document.getElementById('chart1')).resize();
    } else if (chartId === 'chart2') {
      echarts.getInstanceByDom(document.getElementById('chart2')).resize();
    } else if (chartId === 'chart3') {
      echarts.getInstanceByDom(document.getElementById('chart3')).resize();
    } else if (chartId === 'chart4') {
      echarts.getInstanceByDom(document.getElementById('chart4')).resize();
    }
  }, 100); // หน่วงเวลาเล็กน้อยเพื่อให้ DOM โหลดเสร็จก่อน
};


// ข้อมูลของนักเรียนในแต่ละระดับ
const data1 = [
  { value: 0, name: 'ระดับ 1', description: 'ต้องการความช่วยเหลือเร่งด่วน' },
  { value: 0, name: 'ระดับ 2', description: 'ต้องการความช่วยเหลือ' },
  { value: 5, name: 'ระดับ 3', description: 'ต้องการความช่วยเหลือปานกลาง' },
  { value: 10, name: 'ระดับ 4', description: 'อยู่ในสภาพดี' },
  { value: 6, name: 'ระดับ 5', description: 'อยู่ในสภาพดีมาก' }
];

const data2 = [
  { value: 0, name: 'ระดับ 1', description: 'ต้องการความช่วยเหลือเร่งด่วน' },
  { value: 1, name: 'ระดับ 2', description: 'ต้องการความช่วยเหลือ' },
  { value: 4, name: 'ระดับ 3', description: 'ต้องการความช่วยเหลือปานกลาง' },
  { value: 9, name: 'ระดับ 4', description: 'อยู่ในสภาพดี' },
  { value: 7, name: 'ระดับ 5', description: 'อยู่ในสภาพดีมาก' }
];

const data3 = [
  { value: 0, name: 'ระดับ 1', description: 'ต้องการความช่วยเหลือเร่งด่วน' },
  { value: 0, name: 'ระดับ 2', description: 'ต้องการความช่วยเหลือ' },
  { value: 3, name: 'ระดับ 3', description: 'ต้องการความช่วยเหลือปานกลาง' },
  { value: 9, name: 'ระดับ 4', description: 'อยู่ในสภาพดี' },
  { value: 9, name: 'ระดับ 5', description: 'อยู่ในสภาพดีมาก' }
];

const data4 = [
  { value: 0, name: 'ระดับ 1', description: 'ต้องการความช่วยเหลือเร่งด่วน' },
  { value: 1, name: 'ระดับ 2', description: 'ต้องการความช่วยเหลือ' },
  { value: 8, name: 'ระดับ 3', description: 'ต้องการความช่วยเหลือปานกลาง' },
  { value: 11, name: 'ระดับ 4', description: 'อยู่ในสภาพดี' },
  { value: 1, name: 'ระดับ 5', description: 'อยู่ในสภาพดีมาก' }
];

// คำนวณเปอร์เซ็นต์
const data1WithPercentage = data1.map(item => ({ ...item, percentage: ((item.value / totalStudents) * 100).toFixed(2) }));
const data2WithPercentage = data2.map(item => ({ ...item, percentage: ((item.value / totalStudents) * 100).toFixed(2) }));
const data3WithPercentage = data3.map(item => ({ ...item, percentage: ((item.value / totalStudents) * 100).toFixed(2) }));
const data4WithPercentage = data4.map(item => ({ ...item, percentage: ((item.value / totalStudents) * 100).toFixed(2) }));

onMounted(() => {
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
        data: data.map(item => item.name),
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
              const studentData = data.find(item => item.name === params.name);
              return studentData && studentData.value > 0
                ? `${studentData.percentage}%`
                : '';
            },
            textStyle: {
              fontSize: 14,
              color: '#fff',
              fontFamily: 'Inter'
            }
          },
          labelLine: {
            show: false
          },
          data: data,
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

  createChart('chart1', 'สรุปบริบท', ['#C88CFF', '#D8A2FF', '#E6B6FF', '#F0C3FF', '#FBD0FF'], data1WithPercentage);
  createChart('chart2', 'สรุปครอบครัว', ['#6DB9FF', '#79C6FE', '#81D8FF', '#97E6FF', '#ADF2FF'], data2WithPercentage);
  createChart('chart3', 'สรุปนักเรียน', ['#FFA400', '#FFAE2A', '#FFAF3E', '#FFC27E', '#FFD7AA'], data3WithPercentage);
  createChart('chart4', 'สรุปความต้องการ', ['#FF4545', '#FF5757', '#FF8784', '#FFA09D', '#FFABAB'], data4WithPercentage);
});
</script>


<style scoped>
.button-group {
  display: flex;
  justify-content: center;
  margin-top: 30px;
  margin-bottom: 20px;
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
  color: white;
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

table, td {
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