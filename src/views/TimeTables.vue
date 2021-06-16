<template>
  <div class="timetable">
    <div class="table"
    >
      <div
        class="timetable__wrapper"
        v-for="(timetable, key) in groupedTimetables"
        :key="key"
      >
        <h2>{{ `${groupTitle(timetable[0])}` }}</h2>
        <div class="table_wrapper">
          <div class="table_row" v-for="tb in timetable" :key="tb.id">
            <span>{{ moment(tb.start_at).format('HH:mm') }}</span>
            <span>{{ tb.bracket.first_fighter && tb.bracket.first_fighter.name }}</span>
            <span>vs</span>
            <span>{{ tb.bracket.second_fighter && tb.bracket.second_fighter.name }}</span>
          </div>
        </div>  
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
      axios.get(`http://api.kickboxing.beget.tech/api/time-tables/${this.$route.params.ring}`)
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
  mounted() {
    this.timetable()
    setInterval(() => {
      this.timetable()
    }, 10000)
    
  },
  computed: {
    groupedTimetables() {
      const grouped = {}
      this.timetables.forEach((timetable) => {
        if(!grouped[timetable.tournament_bracket_group_id]) {
          grouped[timetable.tournament_bracket_group_id] = []
        }

        grouped[timetable.tournament_bracket_group_id].push(timetable)
        
      })

      return grouped
    }
  }
}
</script>

<style lang="scss" scoped>
.timetable {
    width: 100%;
    height: 100vh;
    display: flex;
    background-color: #DAD2D8;
    overflow: auto;
    .table {
        flex: 1 0 33%;
    }
}

.table {
    width: 25%;
    background: #143642;
    padding: 10px;
    border-right: 1px solid #0F8B8D;
}

.table_wrapper {

}

h2 {
  color: #fff;
}

.table_row {
  background-color: #0F8B8D;
  display: flex;
  padding: 10px 20px;
  justify-content: space-between;
  border-radius: 5px;
  color: #fff;
  margin-bottom: 10px;
}
</style>