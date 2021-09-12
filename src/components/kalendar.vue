<template>
  <div class="kalendar">
    <div class="kalendar__container">
      <div class="kalendar__container-date">
        <div class="kalendar__mounth">
          <div class="kalendar__mounth-container">
          <button @click="changeMonth(-1)">&lt;</button
          > <div class="kalendar__mounth-name">{{ date.MonthName[selectedMonth]
          }}</div><button @click="changeMonth(+1)">&gt;</button>
          </div>
          <div class="kalendar__year">{{ showYear }}</div>
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
              <day
              :day="day">
              </day>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import day from "./day.vue";
import { computed, reactive, ref } from "vue";

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
    const eventsItem = [
      { name: 'Линейка',
      id : 1,
      type : 'highest',
      date: '2021-09-20T15:52:01+03:00'
      },
      { name: 'Линейка',
      id : 2,
      type : 'medium',
      date: '2021-09-20T10:52:01+03:00'
      },
      { name: 'Линейка',
      id : 3,
      type : 'low',
      date: '2021-09-20T13:52:01+03:00'
      },
       { name: 'Линейка',
      id : 4,
      type : 'medium',
      date:'2021-09-22T09:52:01+03:00'
      },
       { name: 'Линейка',
      id : 5,
      type : 'medium',
      date:'2021-09-01T09:52:01+03:00'
      },
       { name: 'Линейка',
      id : 6,
      type : 'low',
      date:'2021-10-15T04:52:01+03:00'
      },
    ]

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
    const showYear = computed(() => {
   
      if(today.getFullYear()==selectedYear.value){
        return ''
      }
      return selectedYear.value
    })
    const eventsCalculate = (fullDdate) => {
      let event = [];
      for (let i = 0 ; i<eventsItem.length-1; i++){
        const fulldateStart = eventsItem[i].date.startsWith(fullDdate);
        if (fulldateStart) {
        const time = new Date(eventsItem[i].date);
        const timeCalculate = time.getHours() + ':'+ time.getMinutes();
        event.push({name:eventsItem[i].name, time:timeCalculate, date : eventsItem[i].date, priority : eventsItem[i].type});
      }
      }  
      event = event.sort(function(a,b){ 
          return  new Date(a.date) - new Date(b.date); 
          });
      return event;

    };

      

    const eventAdd = (year, month, day) => {
      const fullDate = year + "-"+ (month<10? '0'+ month: month )+"-" + (day<10? '0'+ day: day );
      return fullDate;
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
      let firstdayindex = null;
      while (d.getMonth() == selectedMonth.value) {
        if (firstdayindex === null) {
          firstdayindex = d.getDay();
        }
        dayWeek.push({
          day: d.getDate(),
          events: eventsCalculate(
            eventAdd(d.getFullYear(),d.getMonth() + 1,  d.getDate())
          ),
        });
        d = new Date(d.getTime() + 1000 * 60 * 60 * 24);
      }
      const firstdayTimestamp = new Date(
        today.getFullYear(),
        today.getMonth(),
        1
      ).getTime();

      for (let i = 1; i < firstdayindex; i++) {
        const d = new Date(firstdayTimestamp - 1000 * 60 * 60 * 24 * i);
        dayWeek.unshift({
          day: d.getDate(),
          events: eventsCalculate(
            eventAdd(d.getFullYear(),d.getMonth() + 1,  d.getDate())
          ),
        });
      }
      const groups = [];
      for (let i = 0; i < dayWeek.length; i += 7) {
        groups.push(dayWeek.slice(i, i + 7));
      }
      const lastweek = groups[groups.length - 1];
      const lastdayTimestamp = new Date(
        today.getFullYear(),
        today.getMonth(),
        lastweek[lastweek.length - 1].day
      ).getTime();
      const lastweekLenght = lastweek.length;
      if (lastweek.length < 7) {
        for (let i = 1; i <= 7 - lastweekLenght; i++) {
          const d = new Date(lastdayTimestamp + 1000 * 60 * 60 * 24 * i);
          groups[groups.length - 1].push({
            day: d.getDate(),
            events: eventsCalculate(
               eventAdd(d.getFullYear(),d.getMonth() + 1,  d.getDate())
            ),
          });
        }
      }
      date.daynum = groups;
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
      eventAdd,
      eventsItem,
      showYear
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
.kalendar__container-date{
  height: 8%;
}
.kalendar {
  width: 98%;
  height: 98%;
}
.kalendar__container {
  widows: 100%;
  height: 100%;
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
  padding: 20px 0px;
  font-size: 23px;
  font-weight: bold;
margin-left: 20px;
}
.kalendar__table {
  display: flex;
  flex-direction: column;
  height:92%;

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
  padding: 5px 0px;
  height: inherit;
}
.kalendar__table-day {
  width: 100%;
  height: 100%;
  margin: 5px;
  border: 1px solid lightblue;
}
.kalendar__table-column-days {
  flex-direction: column;
height: 100%;
}
.medium{
  background-color:lightgoldenrodyellow;
}
.low{
  background-color:lightgreen;
}
.highest{
  background-color:lightsalmon;
}
.kalendar__mounth-name{
  padding:  0px 30px;
}
.kalendar__year{
  padding-left: 20px;
}
.kalendar__mounth-container{
  width: 250px;
  display: flex;
  justify-content: space-between;
}
</style>
