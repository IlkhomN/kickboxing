<template>
  <div id="app">
    <div class="timetable">
      <div class="table"
        v-for="timetable in timetables"
        :key="timetable.id"
      >
        <h2>{{ `${timetable.combat_sport_type_id} ${timetable.age_group} ${timetable.sex} ${timetable.weight}` }}</h2>
        <div class="table_wrapper">
          <div v-show="i >= 8" class="table_row" v-for="(tb, i) in timetable.tournament_brackets" :key="tb.id">
            <span>{{ tb.start_at }}</span>
            <span>{{ tb.first_fighter && tb.first_fighter.name }}</span>
            <span>vs</span>
            <span>{{ tb.second_fighter && tb.second_fighter.name }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="theme theme-light" id="tbs">
        <div v-if="step !== 5" class="params-selector" style="display: flex; width: 100%; height: 100%; justify-content: center; align-items: center; flex-direction: column;">
            <h1>{{ paramsSelector.title }}</h1>
            <div class="params-wrapper">
                <div
                  @click="selectVariant(variant.value)"
                  style="border: 1px solid rgba(0,0,0,0.3); border-radius: 10px; padding: 5px 10px; margin-bottom: 5px;"
                  v-for="variant in paramsSelector.variants"
                  :key="variant.value"
                  class="param"
                >
                  {{ variant.title }}
                </div>
            </div>
        </div>
        <div v-else-if="reversedTbsGroups.length > 0" class="bracket  disable-image">
            <div
              class="column"
              v-for="tbs in reversedTbsGroups"
              :key="tbs"
            >
                <div
                  class="match winner-top"
                  v-for="tb in tbs_groups[tbs]"
                  :key="tb.id"
                >
                    <div class="match-top team">
                        <span class="image"></span>
                        <span class="seed"><img v-if="tb.first_fighter" :src="`/flags/${tb.first_fighter.country.code}.svg`"></span>
                        <span class="name">{{ tb.first_fighter && tb.first_fighter.name || 'Отсутствует' }}</span>
                        <span class="score">{{ tb.first_fighter && tb.first_fighter.age }}</span>
                    </div>
                    <div class="match-bottom team">
                        <span class="image"></span>
                        <span class="seed"><img v-if="tb.second_fighter" :src="`/flags/${tb.second_fighter.country.code}.svg`"></span>
                        <span class="name">{{ tb.second_fighter && tb.second_fighter.name || 'Отсутствует' }}</span>
                        <span class="score">{{ tb.second_fighter && tb.second_fighter.age }}</span>
                    </div>
                    <div class="match-lines">
                        <div class="line one"></div>
                        <div class="line two"></div>
                    </div>
                    <div class="match-lines alt">
                        <div class="line one"></div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else style="display: flex; width: 100%; height: 80%; justify-content: center; align-items: center; flex-direction: column;">
            <h1>Не найдено!</h1>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import './assets/admin/style.css'
export default {
  data() {
      return {
        tbs_groups: [],
        step: 1,
        selected: {
          sex: null,
          age: null,
          type: null,
          weight: null
        },
        timetables: []
      }
      
  },
  computed: {
      reversedTbsGroups() {
          return Object.keys( this.tbs_groups ).sort( ( a , b ) => b - a);
      },
      paramsSelector() {
        if(this.step === 1) {
            return {
                title: 'Пол',
                variants: [
                    {
                        title: 'Мужской',
                        value: 'male'
                    },
                    {
                        title: 'Женский',
                        value: 'female'
                    },
                ]
            }
        } else if(this.step === 2) {
            return {
                title: 'Возрастная группа',
                variants: [
                    {
                        title: 'Younger Cadets',
                        value: 'younger-cadets'
                    },
                    {
                        title: 'Older Cadets',
                        value: 'older-cadets'
                    },
                    {
                        title: 'Juniors (Tatami)',
                        value: 'juniors'
                    },
                    {
                        title: 'Juniors (Ring)',
                        value: 'younger-juniors'
                    },
                    {
                        title: 'Older Juniors',
                        value: 'older-juniors'
                    },
                    {
                        title: 'Seniors',
                        value: 'seniors'
                    },
                ]
            }
        } else if(this.step === 3) {
            if(['younger-cadets', 'older-cadets', 'juniors'].includes(this.selected.age)) {
                return {
                    title: 'Направление',
                    variants: [
                        {
                            title: 'Light Contact',
                            value: 'light-contact'
                        },
                        {
                            title: 'Kick Light',
                            value: 'kick-light'
                        },
                    ]
                }
            } else if(['seniors'].includes(this.selected.age)) {
                return {
                    title: 'Направление',
                    variants: [
                        {
                            title: 'Full Contact',
                            value: 'full-contact'
                        },
                        {
                            title: 'K-1',
                            value: 'k-1'
                        },
                    ]
                }
            } else {
                return {
                    title: 'Направление',
                    variants: [
                        {
                            title: 'Full Contact',
                            value: 'full-contact'
                        },
                        {
                            title: 'Low-Kick',
                            value: 'low-kick'
                        },
                    ]
                }
            }
        } else if(this.step === 4) {
            if(this.selected.age === 'younger-cadets') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 28',
                                value: '-28'
                            },
                            {
                                title: 'до 32',
                                value: '-32'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 28',
                                value: '-28'
                            },
                            {
                                title: 'до 32',
                                value: '-32'
                            },
                        ]
                    }
                }
            } else if(this.selected.age === 'older-cadets') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 42',
                                value: '-42'
                            },
                            {
                                title: 'до 47',
                                value: '-47'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 42',
                                value: '-42'
                            },
                            {
                                title: 'до 46',
                                value: '-46'
                            },
                        ]
                    }
                }
            } else if(this.selected.age === 'juniors') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 57',
                                value: '-57'
                            },
                            {
                                title: 'до 63',
                                value: '-63'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 50',
                                value: '-50'
                            },
                            {
                                title: 'до 55',
                                value: '-55'
                            },
                        ]
                    }
                }
            } else if(this.selected.age === 'younger-juniors') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 42',
                                value: '-42'
                            },
                            {
                                title: 'до 45',
                                value: '-45'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 36',
                                value: '-36'
                            },
                            {
                                title: 'до 40',
                                value: '-40'
                            },
                        ]
                    }
                }
            } else if(this.selected.age === 'older-juniors') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 51',
                                value: '-51'
                            },
                            {
                                title: 'до 54',
                                value: '-54'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 48',
                                value: '-48'
                            },
                            {
                                title: 'до 52',
                                value: '-52'
                            },
                        ]
                    }
                }
            } else if(this.selected.age === 'seniors') {
                if(this.selected.sex === 'male') {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 51',
                                value: '-51'
                            },
                            {
                                title: 'до 54',
                                value: '-54'
                            },
                        ]
                    }
                } else {
                    return {
                        title: 'Весовая категория',
                        variants: [
                            {
                                title: 'до 48',
                                value: '-48'
                            },
                            {
                                title: 'до 52',
                                value: '-52'
                            },
                        ]
                    }
                }
            } else {
                return {
                    title: 'Весовая категория',
                    variants: [
                        {
                            title: this.selected.sex + this.selected.age + this.selected.type,
                            value: '-51'
                        },
                        {
                            title: 'до 54',
                            value: '-54'
                        },
                    ]
                }
            }
        } else {
            return []
        }
      }
  },
  mounted() {
    this.timetable()
  },
  methods: {
      timetable() {
        axios.get(`http://api.kickboxing.beget.tech/api/tournament-bracket-groups/main`)
          .then(({ data }) => {
              this.timetables = data
          })
      },
      setWinner(tb, winnerId) {
          console.log(tb, winnerId)
        axios.post(`http://api.kickboxing.beget.tech/api/tournament-brackets/${tb.id}/winner`, {
            winner_fighter_id: winnerId
        })
            .then(() => {
                axios.get(`http://api.kickboxing.beget.tech/api/tournament-bracket-groups/${tb.tournament_bracket_group_id}/tournament-brackets`)
                .then(({ data }) => {
                    this.tbs_groups = data
                })
            })
      },
      selectVariant(value) {
        const steps = ['sex', 'age', 'type', 'weight']
        this.selected[steps[this.step-1]] = value

        if(this.step === 4) {
            axios.get(`http://api.kickboxing.beget.tech/api/tournament-bracket-groups/search`, {
                params: this.selected
            })
            .then(({ data }) => {
                this.tbs_groups = data
            })
        }
        this.step++
      }
  }
}
</script>

<style lang="scss">
.timetable {
    width: 100%;
    height: 100vh;
    display: flex;
    background-color: #DAD2D8;
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

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
