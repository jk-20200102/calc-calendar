<template>
    <div>
      <input type="text" v-model="selectedDate" @click="showCalendar" />
      <div v-if="showDatePicker" class="datepicker">
        <div class="datepicker-header">
          <button @click="prevMonth">&lt;</button>
          <span>{{ currentMonth }}</span>
          <button @click="nextMonth">&gt;</button>
        </div>
        <table>
          <thead>
            <tr>
              <th v-for="day in daysOfWeek" :key="day">{{ day }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="week in calendar" :key="week">
              <td v-for="date in week" :key="date"
                @click="selectDate(date)"
                :class="{ 'selected': isDateSelected(date) }"
              >
                {{ date }}
              </td>
            </tr>
          </tbody>
        </table>
        <button @click="hideCalendar">확인</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'DatePicker',
    data() {
      return {
        showDatePicker: false,
        selectedDate: '',
        currentDate: new Date()
      };
    },
    computed: {
      currentMonth() {
        const options = { month: 'long', year: 'numeric' };
        return this.currentDate.toLocaleDateString('ko-KR', options);
      },
      daysOfWeek() {
        const days = ['일', '월', '화', '수', '목', '금', '토'];
        return days.map(day => day.substr(0, 1));
      },
      calendar() {
        const year = this.currentDate.getFullYear();
        const month = this.currentDate.getMonth();
        const firstDay = new Date(year, month, 1).getDay();
        const lastDate = new Date(year, month + 1, 0).getDate();
  
        let date = 1;
        const calendar = [];
  
        for (let week = 0; week < 6; week++) {
          const days = [];
          if (week === 0) {
            for (let i = 0; i < 7; i++) {
              if (i >= firstDay) {
                days.push(date++);
              } else {
                days.push('');
              }
            }
          } else {
            for (let i = 0; i < 7; i++) {
              if (date <= lastDate) {
                days.push(date++);
              } else {
                days.push('');
              }
            }
          }
          calendar.push(days);
        }
  
        return calendar;
      }
    },
    methods: {
      showCalendar() {
        this.showDatePicker = true;
      },
      hideCalendar() {
        this.showDatePicker = false;
      },
      prevMonth() {
        this.currentDate.setMonth(this.currentDate.getMonth() - 1);
      },
      nextMonth() {
        this.currentDate.setMonth(this.currentDate.getMonth() + 1);
      },
      selectDate(date) {
        const month = this.currentDate.getMonth() + 1;
        const formattedDate = `${this.currentDate.getFullYear()}-${month}-${date}`;
        this.selectedDate = formattedDate;
      },
      isDateSelected(date) {
        const month = this.currentDate.getMonth() + 1;
        const formattedDate = `${this.currentDate.getFullYear()}-${month}-${date}`;
        return this.selectedDate === formattedDate;
      }
    }
  };
  </script>
  
  <style>
  .datepicker {
    position: absolute;
    z-index: 1;
    background-color: #fff;
    border: 1px solid #ccc;
    padding: 10px;
  }
  
  .datepicker-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th,
  td {
    padding: 5px;
    text-align: center;
  }
  
  th {
    font-weight: bold;
  }
  
  td {
    cursor: pointer;
  }
  
  td.selected {
    background-color: #ccc;
  }
  </style>
  