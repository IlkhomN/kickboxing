<template>
  <div id="app">
    <div class="theme theme-light" id="tbs" style="min-height: 100vh;">
      <div
        v-if="step !== 5"
        class="params-selector"
        style="display: flex; width: 100%; height: 100%; justify-content: center; align-items: center; flex-direction: column;"
      >
        <h1>{{ paramsSelector.title }}</h1>
        <div class="params-wrapper">
          <div
            @click="selectVariant(variant.value)"
            style="border: 1px solid rgba(0,0,0,0.3); border-radius: 10px; padding: 5px 10px; margin-bottom: 5px;"
            v-for="variant in paramsSelector.variants"
            :key="variant.value"
            class="param"
          >{{ variant.title }}</div>
        </div>
      </div>
      <div
        class="theme__wrapper"
        v-else-if="reversedTbsGroups.length > 0"
        style="    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;"
      >
        <table v-if="step === 5" style="margin-left: 45px; margin-top: 15px; text-align: left;">
          <tr>
            <td>Пол:</td>
            <td style="padding-left: 15px;">
              <a
                href="#"
                @click="(e) => preventSelect(e, 1)"
              >{{ selected.sex === 'male' ? 'Мужской' : 'Женский' }}</a>
            </td>
          </tr>
          <tr>
            <td>Возраст:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 2)">{{ selected.age }}</a>
            </td>
          </tr>
          <tr>
            <td>Направление:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 3)">{{ selected.type }}</a>
            </td>
          </tr>
          <tr>
            <td>Весовая категория:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 4)">{{ selected.weight }}</a>
            </td>
          </tr>
          <tr>
            <td>Действия:</td>
            <td style="padding-left: 15px;">
              <router-link
                :to="{
                  name: 'brackets', params: {
                    sex: selected.sex,
                    age: selected.age,
                    type: selected.type,
                    weight: selected.weight,
                  }
                }"
              >Перейти</router-link>
            </td>
          </tr>
        </table>
        <div class="bracket disable-image">
          <div class="column" v-for="tbs in reversedTbsGroups" :key="tbs">
            <div
              class="match"
              :class="{
                'winner-top': tb.first_fighter_id && tb.winner_fighter_id === tb.first_fighter_id,
                'winner-bottom': tb.second_fighter_id && tb.winner_fighter_id === tb.second_fighter_id,
              }"
              v-for="tb in tbs_groups[tbs]"
              :key="tb.id"
            >
              <div class="match-top team">
                <span class="image"></span>
                <span class="seed">
                  <span
                    style="margin-right: 5px; text-transform: uppercase;"
                    v-if="tb.first_fighter"
                  >{{ tb.first_fighter.country.code }}</span>
                  <img v-if="tb.first_fighter" :src="`/flags/${tb.first_fighter.country.code}.svg`" />
                </span>
                <span class="name">
                  {{ tb.first_fighter && tb.first_fighter.name || 'Отсутствует' }}
                  - {{ tb.first_fighter && tb.first_fighter.city }}
                </span>
                <span class="score">{{ tb.first_fighter && tb.first_fighter.age }}</span>
              </div>
              <div class="match-bottom team">
                <span class="image"></span>
                <span class="seed">
                  <span
                    style="margin-right: 5px; text-transform: uppercase;"
                    v-if="tb.second_fighter"
                  >{{ tb.second_fighter.country.code }}</span>
                  <img
                    v-if="tb.second_fighter"
                    :src="`/flags/${tb.second_fighter.country.code}.svg`"
                  />
                </span>
                <span
                  class="name"
                >
                  {{ tb.second_fighter && tb.second_fighter.name || 'Отсутствует' }}
                  - {{ tb.second_fighter && tb.second_fighter.city }}
                </span>
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
          <div class="column">
            <div
              class="match"
              :class="{
                'winner-top': champion.winner_fighter_id,
              }"
            >
              <div class="match-top team">
                <span class="image"></span>
                <span class="seed">
                  <span
                    style="margin-right: 5px; text-transform: uppercase;"
                    v-if="champion.first_fighter"
                  >{{ champion.first_fighter.country.code }}</span>
                  <img
                    v-if="champion.winner_fighter"
                    :src="`/flags/${champion.winner_fighter.country.code}.svg`"
                  />
                </span>
                <span
                  class="name"
                >{{ champion.winner_fighter && champion.winner_fighter.name || 'Отсутствует' }}</span>
                <span
                  class="score"
                >{{ champion.winner_fighter && `${champion.winner_fighter.age} / ${champion.winner_fighter.weight} кг` }}</span>
              </div>
              <div class="match-lines">
                <div class="line one"></div>
              </div>
              <div class="match-lines alt">
                <div class="line one"></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else
        style="display: flex; width: 100%; height: 80%; justify-content: center; align-items: center; flex-direction: column;"
      >
        <table v-if="step === 5" style="margin-left: 45px; margin-top: 15px; text-align: left;">
          <tr>
            <td>Пол:</td>
            <td style="padding-left: 15px;">
              <a
                href="#"
                @click="(e) => preventSelect(e, 1)"
              >{{ selected.sex === 'male' ? 'Мужской' : 'Женский' }}</a>
            </td>
          </tr>
          <tr>
            <td>Возраст:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 2)">{{ selected.age }}</a>
            </td>
          </tr>
          <tr>
            <td>Направление:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 3)">{{ selected.type }}</a>
            </td>
          </tr>
          <tr>
            <td>Весовая категория:</td>
            <td style="padding-left: 15px;">
              <a href="#" @click="(e) => preventSelect(e, 4)">{{ selected.weight }}</a>
            </td>
          </tr>
        </table>
        <h1 style="display: flex; flex: 1; align-items: center;">Не найдено!</h1>
      </div>
    </div>
    <div class="tts">
      <h1>Расписание</h1>
      <div class="tts_wrapper">
        <router-link target="_blank" :to="{ name: 'time-tables', params: { ring: 'tatami-1' } }">
          <h2>Татами</h2>
        </router-link>
        <router-link target="_blank" :to="{ name: 'time-tables', params: { ring: 'ring-1' } }">
          <h2>Ринг</h2>
        </router-link>
      </div>
    </div>
    <Cities />
    <Fighters />
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import '../assets/admin/style.css'

import Cities from '../components/Cities'
import Fighters from '../components/Fighters'
export default {
  components: {
    Cities, Fighters
  },
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
      timetables: [],
      moment
    }

  },
  computed: {
    champion() {
      return this.tbs_groups[1][0]
    },
    reversedTbsGroups() {
      return Object.keys(this.tbs_groups).sort((a, b) => b - a);
    },
    paramsSelector() {
                if (this.step === 1) {
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
                } else if (this.step === 2) {
                    return {
                    title: 'Возрастная группа',
                    variants: [
                        {
                        title: 'Juniors (9-10)',
                        value: 'juniors'
                        },
                        {
                        title: 'Younger Juniors (11-12)',
                        value: 'younger-juniors'
                        },
                        {
                        title: 'Older Juniors (13-14)',
                        value: 'older-juniors'
                        },
                        {
                        title: 'Seniors (15-16)',
                        value: 'seniors'
                        },
                    ]
                    }
                } else if (this.step === 3) {
                    if (['juniors'].includes(this.selected.age)) {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Kick Light',
                            value: 'kick-light'
                        },
                        ]
                    }
                    } else if (['younger-juniors'].includes(this.selected.age)) {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Kick Light',
                            value: 'kick-light'
                        },
                        ]
                    }
                    }else if (['seniors'].includes(this.selected.age)) {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Low Kick',
                            value: 'low-kick'
                        },
                        ]
                    }
                    } else {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Low-Kick',
                            value: 'low-kick'
                        },
                        ]
                    }
                    }
                } else if (this.step === 4) {
                    if (this.selected.age === 'younger-cadets') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 22',
                            value: '-22'
                            },{
                            title: 'до 25',
                            value: '-25'
                            },
                            {
                            title: 'до 27',
                            value: '-27'
                            },
                            {
                            title: 'до 29',
                            value: '-29'
                            },
                            {
                            title: 'до 31',
                            value: '-31'
                            },
                            {
                            title: 'до 33',
                            value: '-33'
                            },
                            {
                            title: 'до 35',
                            value: '-35'
                            },
                            {
                            title: 'до 37',
                            value: '-37'
                            },
                            {
                            title: 'до 40',
                            value: '-40'
                            },
                            {
                            title: 'до 43',
                            value: '-43'
                            },
                            {
                            title: 'от 43',
                            value: '43'
                            },
                        ]
                        }
                    } else {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 24',
                            value: '-24'
                            },
                            {
                            title: 'до 27',
                            value: '-27'
                            },
                            {
                            title: 'до 33',
                            value: '-33'
                            },
                        ]
                        }
                    }
                    } else if (this.selected.age === 'older-cadets') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 30',
                            value: '-30'
                            },
                            {
                            title: 'до 32',
                            value: '-32'
                            },
                            {
                            title: 'до 34',
                            value: '-34'
                            },
                            {
                            title: 'до 36',
                            value: '-36'
                            },
                            {
                            title: 'до 38',
                            value: '-38'
                            },
                            {
                            title: 'до 40',
                            value: '-40'
                            },
                            {
                            title: 'до 42',
                            value: '-42'
                            },
                            {
                            title: 'до 44',
                            value: '-44'
                            },
                            {
                            title: 'до 46',
                            value: '-46'
                            },
                            {
                            title: 'от 46',
                            value: '46'
                            },
                        ]
                        }
                    } else {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 24',
                            value: '-24'
                            },
                            {
                            title: 'до 27',
                            value: '-27'
                            },
                            {
                            title: 'до 30',
                            value: '-30'
                            },
                            {
                            title: 'до 33',
                            value: '-33'
                            },
                            {
                            title: 'до 36',
                            value: '-36'
                            },
                        ]
                        }
                    }
                    } else if (this.selected.age === 'juniors') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 22',
                            value: '-22'
                            },{
                            title: 'до 25',
                            value: '-25'
                            },
                            {
                            title: 'до 27',
                            value: '-27'
                            },
                            {
                            title: 'до 29',
                            value: '-29'
                            },
                            {
                            title: 'до 31',
                            value: '-31'
                            },
                            {
                            title: 'до 33',
                            value: '-33'
                            },
                            {
                            title: 'до 35',
                            value: '-35'
                            },
                            {
                            title: 'до 37',
                            value: '-37'
                            },
                            {
                            title: 'до 40',
                            value: '-40'
                            },
                            {
                            title: 'до 43',
                            value: '-43'
                            },
                            {
                            title: 'от 43',
                            value: '43'
                            },
                        ]
                        }
                    } else {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 33',
                            value: '-33'
                            },
                            {
                            title: 'до 38',
                            value: '-38'
                            },
                            {
                            title: 'до 42',
                            value: '-42'
                            },
                            {
                            title: 'от 47',
                            value: '47'
                            },
                        ]
                        }
                    }
                    } else if (this.selected.age === 'younger-juniors') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 30',
                            value: '-30'
                            },
                            {
                            title: 'до 32',
                            value: '-32'
                            },
                            {
                            title: 'до 34',
                            value: '-34'
                            },
                            {
                            title: 'до 36',
                            value: '-36'
                            },
                            {
                            title: 'до 38',
                            value: '-38'
                            },
                            {
                            title: 'до 40',
                            value: '-40'
                            },
                            {
                            title: 'до 42',
                            value: '-42'
                            },
                            {
                            title: 'до 44',
                            value: '-44'
                            },
                            {
                            title: 'до 46',
                            value: '-46'
                            },
                            {
                            title: 'от 46',
                            value: '46'
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
                            title: 'до 47',
                            value: '-47'
                            },
                            {
                            title: 'до 52',
                            value: '-52'
                            },
                            {
                            title: 'от 57',
                            value: '57'
                            },
                        ]
                        }
                    }
                    } else if (this.selected.age === 'older-juniors') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 35',
                            value: '-35'
                            },
                            {
                            title: 'до 37',
                            value: '-37'
                            },
                            {
                            title: 'до 39',
                            value: '-39'
                            },
                            {
                            title: 'до 41',
                            value: '-41'
                            },
                            {
                            title: 'до 43',
                            value: '-43'
                            },
                            {
                            title: 'до 45',
                            value: '-45'
                            },
                            {
                            title: 'до 47',
                            value: '-47'
                            },
                            {
                            title: 'до 49',
                            value: '-49'
                            },
                            {
                            title: 'до 51',
                            value: '-51'
                            },
                            {
                            title: 'до 54',
                            value: '-54'
                            },
                            {
                            title: 'до 57',
                            value: '-57'
                            },
                            {
                            title: 'от 57',
                            value: '57'
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
                            {
                            title: 'до 57',
                            value: '-57'
                            },
                            {
                            title: 'до 62',
                            value: '-62'
                            },
                        ]
                        }
                    }
                    } else if (this.selected.age === 'seniors') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 40',
                            value: '-40'
                            },{
                            title: 'до 42',
                            value: '-42'
                            },
                            {
                            title: 'до 45',
                            value: '-45'
                            },
                            {
                            title: 'до 48',
                            value: '-48'
                            },
                            {
                            title: 'до 51',
                            value: '-51'
                            },
                            {
                            title: 'до 54',
                            value: '-54'
                            },
                            {
                            title: 'до 57',
                            value: '-57'
                            },
                            {
                            title: 'до 60',
                            value: '-60'
                            },
                            {
                            title: 'до 63.5',
                            value: '-63.5'
                            },
                            {
                            title: 'до 67',
                            value: '-67'
                            },
                            {
                            title: 'до 71',
                            value: '-71'
                            },
                            {
                            title: 'от 71',
                            value: '71'
                            },
                        ]
                        }
                    } else {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 52',
                            value: '-52'
                            },
                            {
                            title: 'до 57',
                            value: '-57'
                            },
                            {
                            title: 'до 62',
                            value: '-62'
                            },
                            {
                            title: 'от 67',
                            value: '67'
                            },
                        ]
                        }
                    }
                    } else {
                    return {
                        title: 'Неизвестная ошибка',
                        variants: [
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
    setInterval(() => {
      if (this.selected.weight) {
        axios.get(`${process.env.VUE_APP_API}/tournament-bracket-groups/search`, {
          params: this.selected
        })
          .then(({ data }) => {
            this.tbs_groups = data
          })
      }
    }, 10000);
  },
  methods: {
    preventSelect(e, step) {
      e.preventDefault();
      this.step = step
    },
    timetable() {
      axios.get(`${process.env.VUE_APP_API}/time-tables/ring-3`)
        .then(({ data }) => {
          this.timetables = data
        })
    },
    setWinner(tb, winnerId) {
      console.log(tb, winnerId)
      axios.post(`${process.env.VUE_APP_API}/tournament-brackets/${tb.id}/winner`, {
        winner_fighter_id: winnerId
      })
        .then(() => {
          axios.get(`${process.env.VUE_APP_API}/tournament-bracket-groups/${tb.tournament_bracket_group_id}/tournament-brackets`)
            .then(({ data }) => {
              this.tbs_groups = data
            })
        })
    },
    selectVariant(value) {
      const steps = ['sex', 'age', 'type', 'weight']
      this.selected[steps[this.step - 1]] = value

      if (this.step === 4) {
        axios.get(`${process.env.VUE_APP_API}/tournament-bracket-groups/search`, {
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
body {
  margin: 0;
  height: auto;
}
.timetable {
  width: 100%;
  height: 100vh;
  display: flex;
  background-color: #dad2d8;
  overflow: auto;
  .table {
    flex: 1 0 33%;
  }
}

.table {
  width: 25%;
  background: #143642;
  padding: 10px;
  border-right: 1px solid #0f8b8d;
}

.table_wrapper {
}

h2 {
  color: #fff;
}

.bracket .name {
  font-size: 12px;
}

.table_row {
  background-color: #0f8b8d;
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

.theme {
  display: flex;
  flex-direction: column;
  //   height: 100vh;
}
.params-selector {
  flex: 1;
  .params-wrapper {
    flex: 1;
    display: flex;
    align-items: center;
    .param {
      margin: 0 15px;
    }
  }
}

.tts {
  height: 100vh;
  box-sizing: border-box;
  padding: 90px;
  text-align: center;
  h1 {
    text-align: center;
  }
  h2 {
    color: #000;
    margin-top: 90px;
  }
  a {
    display: block;
    font-size: 16px;
    text-transform: uppercase;
    color: #000;
    text-decoration: none;
    &:hover {
      color: #143642;
    }
  }
}
</style>
