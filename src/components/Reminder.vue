<template>
    <Layout>
      <div class="reminder-nav-right">
        <div class="reminder-header reminder-row flex-middle">
          <div class="reminder-col reminder-col-start">
            <div class="reminder-icon reminder-icon-left" @click="prevMonth"></div>
          </div>
          <div class="reminder-col reminder-col-center">
            <span class="reminder-date">{{ formattedCurrentDate }}</span>
          </div>
          <div class="reminder-col reminder-col-end">
            <div class="reminder-icon reminder-icon-right" @click="nextMonth"></div>
          </div>
        </div>
        <div class="reminder-days reminder-row">
          <div
            class="reminder-col reminder-col-center"
            v-for="(day, index) in daysOfWeek"
            :key="index"
          >
            {{ day }}
          </div>
        </div>
        <div class="body">
          <div
            class="reminder-row"
            v-for="(row, rowIndex) in calendarRows"
            :key="rowIndex"
          >
            <div
              class="reminder-col reminder-cell"
              v-for="(day, colIndex) in row"
              :key="colIndex"
            >
              <span class="reminder-number">{{ day.formattedDate }}</span>
              <textarea
                class="reminder-textarea"
                v-model="reminders[day.fullDate]"
                @input="saveReminders"
                placeholder=""
              />
            </div>
          </div>
        </div>
      </div>
    </Layout>
  </template>
  
  <script>
  import Layout from "../../components/Layout.vue";
  import {
    format,
    startOfMonth,
    endOfMonth,
    startOfWeek,
    endOfWeek,
    addDays,
    addMonths,
    subMonths,
  } from "date-fns";
  
  export default {
    name: "Reminder",
    components: {
      Layout,
    },
    data() {
      return {
        currentDate: new Date(),
        reminders: {},
      };
    },
    computed: {
      formattedCurrentDate() {
        return format(this.currentDate, "MMMM yyyy");
      },
      daysOfWeek() {
        const startDate = startOfWeek(this.currentDate);
        return Array.from({ length: 7 }).map((_, i) =>
          format(addDays(startDate, i), "EEEE")
        );
      },
      calendarRows() {
        const monthStart = startOfMonth(this.currentDate);
        const monthEnd = endOfMonth(monthStart);
        const startDate = startOfWeek(monthStart);
        const endDate = endOfWeek(monthEnd);
  
        let day = startDate;
        const rows = [];
        while (day <= endDate) {
          const row = [];
          for (let i = 0; i < 7; i++) {
            const fullDate = format(day, "yyyy-MM-dd");
            row.push({
              formattedDate: format(day, "d"),
              fullDate,
            });
            day = addDays(day, 1);
          }
          rows.push(row);
        }
        return rows;
      },
    },
    mounted() {
      const savedReminders = localStorage.getItem("reminders");
      if (savedReminders) {
        this.reminders = JSON.parse(savedReminders);
      }
    },
    methods: {
      saveReminders() {
        localStorage.setItem("reminders", JSON.stringify(this.reminders));
      },
      nextMonth() {
        this.currentDate = addMonths(this.currentDate, 1);
      },
      prevMonth() {
        this.currentDate = subMonths(this.currentDate, 1);
      },
    },
  };
  </script>
  
  <style scoped>
  header {
    line-height: 1.5;
  }
  
  .logo {
    display: block;
    margin: 0 auto 2rem;
  }
  
  @media (min-width: 1024px) {
    header {
      display: flex;
      place-items: center;
      padding-right: calc(var(--section-gap) / 2);
    }
  
    .logo {
      margin: 0 2rem 0 0;
    }
  
    header .wrapper {
      display: flex;
      place-items: flex-start;
      flex-wrap: wrap;
    }
  }
  </style>
  