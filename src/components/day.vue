<template>
  <div class="days-item">
    Date : {{ date }} <br />
    Day: {{ day }}
  </div>
</template>

<script>
//import { reactive } from "vue";
export default {
  name: "day",
  props: {
    date: {
      type: Number,
    },
    day: {
      type: Number,
    },
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
</style>
