<template>
  <div>
    <div>{{getDate}}</div>
    <table>
      <!-- header row -->
      <tr>
        <th v-for="week in weekList" v-bind:key="week" v-once>
          {{week}}
        </th>
      </tr>
      <tr v-for="(dayRow, index) in dayList()" v-bind:key="index" v-once>
        <td v-for="day in dayRow" v-bind:key="day" v-once>
          <span v-if="day > 0" v-once v-bind:class="{ currentDay: day === currentDay}">{{day}}</span>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Calendar',
  data: () => {
    return {
      weekList: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
      dayList: () => {
        const dayList = [];
        let currentDayIndex = 1;
        const date = new Date();
        const year = date.getFullYear()
        const month = date.getMonth();
        const totalDaysInCurrentMonth = new Date(year, month + 1, 0).getDate();
        while (currentDayIndex <= totalDaysInCurrentMonth) {
          const dayRow = [];
          if (currentDayIndex == 1) {
            const firstDayInWeek = new Date(year, month, 1).getDay();
            for (let i = 0; i < firstDayInWeek; i++) {
              dayRow.push(-i);
            }
          }
          while (dayRow.length < 7 && currentDayIndex <= totalDaysInCurrentMonth) {
            dayRow.push(currentDayIndex);
            currentDayIndex++;
          }
          dayList.push(dayRow);
        }
        return dayList;
      }
    }
  },
  computed: {
    getDate() {
      const d = new Date();
      return d.toDateString()
    },
    currentDay: () => {
      return new Date().getDate();
    },
  },
  methods: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.currentDay {
  background-color: blue;
}
</style>
