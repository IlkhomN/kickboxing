<template>
  <div id="app">
    <div class="theme theme-light" id="tbs">
        <h1>КИКБОКСИНГ СПОРТ ТУРИ БУЙИЧА 2013-2008 ЙИЛЛАРДА ТУГУЛГАН УГИЛ ВА КИЗ БОЛЛАР УРТАСИДА УЗБЕКИСТОН КУБОГИ</h1>
        <div class="theme__wrapper" v-if="reversedTbsGroups.length > 0"  style="    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;">
          <div class="data_left-wrapper">
            <div class="bracket  disable-image">
                <div
                  class="column"
                  v-for="tbs in reversedTbsGroups"
                  :key="tbs"
                >
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
                              >
                                {{tb.first_fighter.country.code}}
                              </span>
                              <img v-if="tb.first_fighter" :src="`/flags/${tb.first_fighter.country.code}.svg`">
                              </span>
                            <span class="name">{{ tb.first_fighter && tb.first_fighter.name || 'Отсутствует' }}</span>
                            <span class="score">{{ tb.first_fighter && tb.first_fighter.age }}</span>
                        </div>
                        <div class="match-bottom team">
                            <span class="image"></span>
                            <span class="seed">
                              <span
                                style="margin-right: 5px; text-transform: uppercase;"
                                v-if="tb.second_fighter"
                              >
                                {{tb.second_fighter.country.code}}
                              </span>
                              <img v-if="tb.second_fighter" :src="`/flags/${tb.second_fighter.country.code}.svg`">
                            </span>
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
                              >
                                {{champion.first_fighter.country.code}}
                              </span>
                              <img v-if="champion.winner_fighter" :src="`/flags/${champion.winner_fighter.country.code}.svg`">
                            </span>
                            <span class="name">{{ champion.winner_fighter && champion.winner_fighter.name || 'Отсутствует' }}</span>
                            <span class="score">{{ champion.winner_fighter && `${champion.winner_fighter.age} / ${champion.winner_fighter.weight} кг` }}</span>
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
            <div class="sign-place">
              <p>Главный судья: _______________</p>
              <p>Главный секретарь: ____________</p>
              <p>Секретарь: ____________</p>
            </div>
          </div>

          <div class="data__right-wrapper">
            <table style="margin-left: 45px; margin-top: 15px; text-align: left;">
                <tr>
                    <td>
                        Пол:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.sex === 'male' ? 'Мужской' : 'Женский' }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Возраст:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.age }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Направление:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.type }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Весовая категория:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.weight }}</a>
                    </td>
                </tr>
            </table>

            <table style="margin-left: 45px; margin-top: 15px; text-align: left;">
                <tr v-for="(winner, i) in tbs_groups[1][0].group.winners" :key="winner">
                    <td>
                        {{i+1}}.  {{ winner }}
                    </td>
                </tr>
            </table>
          </div>
          
           
        </div>
        
        <div v-else style="display: flex; width: 100%; height: 80%; justify-content: center; align-items: center; flex-direction: column;">
            <table style="margin-left: 45px; margin-top: 15px; text-align: left;">
                <tr>
                    <td>
                        Пол:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.sex === 'male' ? 'Мужской' : 'Женский' }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Возраст:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.age }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Направление:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.type }}</a>
                    </td>
                </tr>
                <tr>
                    <td>
                        Весовая категория:
                    </td>
                    <td style="padding-left: 15px;">
                        <a href="#">{{ $route.params.weight }}</a>
                    </td>
                </tr>
            </table>
            <h1 style="display: flex; flex: 1; align-items: center;">Не найдено!</h1>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import '../assets/admin/style.css'
export default {
  data() {
      return {
        tbs_groups: [],
        timetables: [],
        moment
      }
      
  },
  computed: {
      champion() {
        return this.tbs_groups[1][0]
      },
      reversedTbsGroups() {
          return Object.keys( this.tbs_groups ).sort( ( a , b ) => b - a);
      },
  },
  mounted() {
    setInterval(() => {
            axios.get(`https://wako-api.interactive-labs.ru/api/tournament-bracket-groups/search`, {
                params: this.$route.params
            })
            .then(({ data }) => {
                this.tbs_groups = data
            })
    }, 2000);
  },
}
</script>

<style lang="scss">
body {
  margin: 0;
  height: auto;
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
  height: 100vh;
}
.theme__wrapper {
  flex-direction: row !important;
      height: 100%;
    padding-bottom: 60px;
}

.data_left-wrapper {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.sign-place {
  display: flex;
  margin-top: 60px;
  p {
    margin-right: 150px;
  }
}

.bracket {
    margin-left: 0;
    padding: 0;
    flex: 1;
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

.data__right-wrapper {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
