<template>
  <div class="timetable">
    <div class="table"
    >
      <div
        class="timetable__wrapper"
      >
        <h1 style="text-align: center;">{{ $route.params.ring.toUpperCase() }}</h1>
        <table class="time-table">
          <thead>
            <th>Время</th>
            <th>Бойец</th>
            <th></th>
            <th>Бойец</th>
          </thead>
          <tbody>
            <template v-for="(tb, i) in timetables">
              <tr class="header" :class="[`group-${tb.group.id}`]" v-if="i == 0 || timetables[i-1].group.id !== tb.group.id" :key="tb.id">
                <td colspan="4">{{ `${groupTitle(tb)}` }}</td>
              </tr>
              <tr :key="tb.id">
                <td>{{ i+1 }}</td>
                <td>{{ tb.bracket.second_fighter && `${tb.bracket.second_fighter.country.name} -  ${tb.bracket.second_fighter.name}` }}</td>
                <td>vs</td>
                <td>{{ tb.bracket.first_fighter && `${tb.bracket.first_fighter.country.name} -  ${tb.bracket.first_fighter.name}` }}</td>
              </tr>  
            </template>
            
          </tbody>
        </table>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  data() {
    return {
      timetables: [],
      moment
    }
  },
  methods: {
    timetable() {
      axios.get(`https://wako-api.interactive-labs.ru/api/time-tables/${this.$route.params.ring}`)
        .then(({ data }) => {
            this.timetables = data
        })
    },
    groupTitle(tb) {
      const {group} = tb
      const sexTitle = group.sex === 'male' ? 'Мужской' : 'Женский'
      return `${sexTitle} ${ group.age_group_name } ${group.type.name} ${group.weight_name} 1/${tb.final}`
    }

  },
  computed: {
    timetablesInner() {
      return this.timetables.filter((
        // timetable
        ) => {
        // return timetable.group.combat_sport_type_id === 2 && timetable.final === 16
        return true
      }).sort((a,b) => {
        if(a.group.weight < b.group.weight) {
          return 1
        } else if(a.group.weight > b.group.weight) {
          return -1
        } else {
          return 0
        }
      })
    }
  },
  mounted() {
    this.timetable()
    setInterval(() => {
      this.timetable()
    }, 10000)
    
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: #fff;
}

* {
  box-sizing: border-box;
  color: #000;
}
.timetable {
    width: 100%;
    display: flex;
    background-color: #fff;
    overflow: auto;
    .table {
        flex: 1 0 33%;
    }
}

.table {
    width: 25%;
    // background: #143642;
    padding: 10px;
    border-right: 1px solid #0F8B8D;
}

.time-table {
  width: 100%;
  thead {
    // background-color: #034344;
    padding: 10px 20px;
    border-radius: 5px;
    color: #fff;
    margin-bottom: 10px;
  }
  tr {
    // background-color: #0F8B8D;
    padding: 10px 20px;
    border-radius: 5px;
    color: #fff;
    margin-bottom: 10px;
    &.header {
      // background-color: #005e5f;
      font-size: 20px;
      font-family: sans-serif;
      page-break-before: always;
      td {
        text-align: center;;
      }
    }
  }
  td, th {
    padding: 10px 15px;
    font-weight: 600;
    // font-size: 20px;
    font-family: sans-serif;
  }
}

h2 {
  font-family: sans-serif;
}

.table_wrapper {

}

h2 {
  color: #fff;
}

.table_row {
  // background-color: #0F8B8D;
  display: flex;
  padding: 10px 20px;
  justify-content: space-between;
  border-radius: 5px;
  color: #fff;
  margin-bottom: 10px;
}

@media (max-width: 256px) {
  #app * {
    font-size: 10px !important;
  }

  .table {
    padding: 0px;
    box-sizing: border-box;
  }

  thead {
    padding: 2px 5px;
    margin-bottom: 2px;
  }
  tr {
    padding: 2px 5px;
    margin-bottom: 2px;
    &.header {
      font-size: 100px;
    }
  }
  td, th {
    padding: 2px 5px;
  }
  .time-table td, .time-table th {
      padding: 5px 5px;
  }
}
</style>