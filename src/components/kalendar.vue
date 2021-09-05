<template>
  <div class="kalendar">
    <div class="kalendar__container">
      <div class="kalendar__container-date">
        <div class="kalendar__mounth">
          <!-- <select v-model="selectedMonth" @change="getDates">
            <option
              v-for="(mounth, index) in date.MonthName"
              :key="index"
              :value="index"
            >
              {{ mounth }}
            </option>
          </select> -->
          <button @click="changeMonth(-1)">&lt;</button
          >{{ date.MonthName[selectedMonth]
          }}<button @click="changeMonth(+1)">&gt;</button>
          <div class="kalendar__year">{{ selectedYear }}</div>
        </div>
      </div>
      <div class="kalendar__table">
        <div class="kalendar__table-column">
          <div
            class="kalendar__table-name-date"
            v-for="(day, index) in date.dayName"
            :key="index"
          >
            {{ day.name }}
          </div>
        </div>
        <div class="kalendar__table-column kalendar__table-column-days">
          <div
            v-for="(d, indx) in date.daynum"
            :key="indx"
            class="kalendar__table-week"
          >
            <div v-for="day in d" :key="day.day" class="kalendar__table-day">
              {{ day.day }}
              <ul>
                <li v-for="(item, ind) in day.events" :key="ind">{{ item }}</li>
              </ul>
            </div>
          </div>
        </div>
        <div class="kalendar__table-column kalendar_max-width-day">
          <day
            v-for="item in dayArray"
            :date="item.date"
            :day="item.day"
            :key="item.date"
          ></day>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import day from "./day.vue";
import { reactive, ref } from "vue";

export default {
  components: { day },
  name: "kalendar",
  setup() {
    const date = reactive({
      dayName: [
        {
          name: "Понедельник",
        },
        {
          name: "Вторник",
        },
        {
          name: "Среда",
        },
        {
          name: "Четверг",
        },
        {
          name: "Пятница",
        },
        {
          name: "Суббота",
        },
        {
          name: "Воскресенье",
        },
      ],
      MonthName: [
        "Январь",
        "Февраль",
        "Март",
        "Апрель",
        "Май",
        "Июнь",
        "Июль",
        "Август",
        "Сентябрь",
        "Октябрь",
        "Ноябрь",
        "Декабрь",
      ],
      daynum: [],
    });
    const events = {
      "8.9.2021": ["10.00 - линейка"],
      "12.9.2021": ["10.00 - линейка", "12.00-физкультура"],
      "21.9.2021": ["10.00 - линейка", "12.00-физкультура"],
      "30.9.2021": ["10.00 - линейка", "12.00-физкультура"],
      "1.10.2021": ["10.00 - линейка", "12.00-физкультура"],
      "5.10.2021": ["10.00 - линейка", "12.00-физкультура"],
      "5.11.2021": ["10.00 - линейка", "12.00-физкультура"],
      "9.11.2021": ["10.00 - линейка", "12.00-физкультура"],
    };
    // for (let i = 0; i < 7; i++) {
    //   dayWeek.value.push('');
    // }
    const today = new Date();
    const selectedMonth = ref(today.getMonth());
    const selectedDay = ref(today.getDate());
    const selectedYear = ref(today.getFullYear());
    const changeMonth = (calculateMonth) => {
      let month = selectedMonth.value + calculateMonth;
      if (month === 12) {
        selectedMonth.value = 0;
        selectedYear.value = selectedYear.value + 1;
      } else if (month === -1) {
        selectedMonth.value = 11;
        selectedYear.value = selectedYear.value - 1;
      } else {
        selectedMonth.value = month;
      }
      getDates();
    };
    const eventsCalculate = (fullDdate) => {
      let event = [];
      if (events[fullDdate]) {
        event = events[fullDdate];
      }
      return event;
    };
    const getDates = () => {
      date.daynum = [];
      let d = new Date(selectedYear.value, selectedMonth.value);
      const dayWeek = [];
      const prevMonthData = {
        month: selectedMonth.value - 1,
        year: selectedYear.value,
      };

      if (selectedMonth.value === 0) {
        prevMonthData.month = 11;
        prevMonthData.year = selectedYear.value - 1;
      }

      let prevMonth = new Date(prevMonthData.year, prevMonthData.month);
      const prevMonthDays = [];
      while (prevMonth.getMonth() == prevMonthData.month) {
        prevMonthDays.push(prevMonth.getDate());
        prevMonth = new Date(prevMonth.getTime() + 1000 * 60 * 60 * 24);
      }
      let firstdayindex = null;
      while (d.getMonth() == selectedMonth.value) {
        if (firstdayindex === null) {
          firstdayindex = d.getDay();
        }

        const fullDdate =
          String(d.getDate()) +
          "." +
          String(d.getMonth() + 1) +
          "." +
          String(d.getFullYear());
          

        dayWeek.push({ day: d.getDate(), events: eventsCalculate(fullDdate) });
        d = new Date(d.getTime() + 1000 * 60 * 60 * 24);
      }
      for (let i = 1; i < firstdayindex; i++) {
          const fullDdate =
          String(prevMonthDays[prevMonthDays.length - i]) +
          "." +
          String(prevMonthData.month+1) +
          "." +
          String(prevMonthData.year);
        dayWeek.unshift({day: prevMonthDays[prevMonthDays.length - i], events: eventsCalculate(fullDdate)});
      }
      const groups = [];
      for (let i = 0; i < dayWeek.length; i += 7) {
        groups.push(dayWeek.slice(i, i + 7));
      }
      const lastweek = groups[groups.length - 1];
      const nextMonth = {month:selectedMonth.value+1, year: selectedYear.value}
       if (selectedMonth.value === 11) {
        nextMonth.month = 0;
        nextMonth.year = selectedYear.value + 1;
      }
      if (lastweek.length < 7) {
        for (let i = lastweek.length, j = 1; i < 7; i++, j++) {
           const fullDdate =
          String(j) +
          "." +
          String(nextMonth.month+1) +
          "." +
          String(nextMonth.year);
          groups[groups.length - 1].push({day:j, events: eventsCalculate(fullDdate)});
        }
      }
      date.daynum = groups;
      console.log(dayWeek);
    };

    getDates();
    return {
      date,
      selectedMonth,
      selectedDay,
      selectedYear,
      today,
      getDates,
      changeMonth,
      eventsCalculate,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.kalendar_max-width-day {
  flex-wrap: wrap;
}
.kalendar {
  width: 100%;
}
.kalendar__container {
  widows: 100%;
  padding: 20px;
  border-radius: 10px;
  border: 1px solid grey;
}
.kalendar__img {
  width: 20px;
  height: 20px;
  transform: rotate(180deg);
}
.kalendar__mounth {
  display: flex;
}
.kalendar__table {
  display: flex;
  flex-direction: column;
}
.kalendar__table-column {
  display: flex;
  justify-content: space-between;
}
.kalendar__table-name-date {
  width: 14%;
}
.kalendar__table-week {
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 20px 0px;
}
.kalendar__table-day {
  width: 14%;
  height: 100%;
}
.kalendar__table-column-days {
  flex-direction: column;
}
</style>
