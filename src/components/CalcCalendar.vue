<template>
    <div>
      <input type="text" v-model="selectedDate" @click="showCalendar" />
      <div v-if="showDatePicker">
        <!-- 달력 UI 구현 -->
        <DatePicker></DatePicker>
        <!-- 날짜 선택 시 selectedDate 업데이트 -->
        <!-- "확인" 버튼 클릭 시 hideCalendar 메소드 호출 -->
      </div>
    </div>
  </template>
  
  <script>
//   import Calendar from './Calendar.vue';
  import DatePicker from './DatePicker.vue';

  export default {
    components: {
        DatePicker,
    },
    data() {
      return {
        showDatePicker: false,
        selectedDate: ''
      };
    },
    methods: {
      showCalendar() {
        this.showDatePicker = true;
      },
      hideCalendar() {
        this.showDatePicker = false;
      }
    }
  };
  </script>
  