<template>
<div >
              {{ day.day }}
              <ul class="kalendar__table-day-list">
                <li v-for="(item, ind) in day.events" :key="ind" :class="item.priority">{{ item.name }} {{item.time}}</li>
              </ul>
            </div>
</template>

<script>
//import { reactive } from "vue";
export default {
  name: "day",
  props: {
    day: {}
  },
  setup() {
    return {};
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
.days-item {
  width: 14%;
  display: flex;
  justify-content: space-between;
}
.days-item_border {
  border: 1px solid lightblue;
  border-radius: 10px;
  width: 14%;
}
.kalendar__table-day {
  width: 100%;
  height: 100%;
}
.kalendar__table-day-list{
  display: flex;
  flex-direction: column;
}
</style>
