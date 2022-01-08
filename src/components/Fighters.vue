<template>
  <div class="fighters" id="fighters">
    <h1>Просмотр бойцов</h1>
    <div v-if="showSelector" class="fighters__selector">
      <h1>{{ paramsSelector.title }}</h1>
      <div class="fighters__selector-wrapper">
        <div
          @click="selectVariant(variant.value)"
          style="border: 1px solid rgba(0,0,0,0.3); border-radius: 10px; padding: 5px 10px; margin-bottom: 5px;"
          v-for="variant in paramsSelector.variants"
          :key="variant.value"
          class="param"
        >{{ variant.title }}</div>
      </div>
    </div>
    <div v-else class="fighters__list-wrapper">
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
      <div class="fighters__list">
        <div
          v-for="fighter in fighters"
          :key="fighter.id"
          class="fighters__fighter"
        >{{ fighter.name }} - {{ fighter.age }} лет / {{ fighter.weight }} кг. {{ fighter.city }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      step: 1,
      selected: {
        sex: null,
        age: null,
        type: null,
        weight: null
      },
      fighters: []
    }
  },

  computed: {
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
              title: 'Juniors (13-14)',
              value: 'juniors'
            },
            {
              title: 'Younger Juniors (15-16)',
              value: 'younger-juniors'
            },
            {
              title: 'Older Juniors (17-18)',
              value: 'older-juniors'
            },
            {
              title: 'Seniors (19-35)',
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
        } else if (['seniors'].includes(this.selected.age)) {
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
                  title: 'до 21',
                  value: '-21'
                },
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
                {
                  title: 'от 36',
                  value: '36'
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
                {
                  title: 'до 39',
                  value: '-39'
                },
                {
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
                  title: 'от 48',
                  value: '48'
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
                  title: 'до 33',
                  value: '-33'
                },
                {
                  title: 'до 36',
                  value: '-36'
                },
                {
                  title: 'до 39',
                  value: '-39'
                },
                {
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
                  title: 'от 63.5',
                  value: '63.5'
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
                  title: 'до 75',
                  value: '-75'
                },
                {
                  title: 'от 75',
                  value: '75'
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
                  title: 'до 45',
                  value: '-45'
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
                  title: 'до 75',
                  value: '-75'
                },
                {
                  title: 'от 75',
                  value: '75'
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
                  title: 'до 75',
                  value: '-75'
                },
                {
                  title: 'до 81',
                  value: '-81'
                },
                {
                  title: 'до 86',
                  value: '-86'
                },
                {
                  title: 'до 91',
                  value: '-91'
                },
                {
                  title: 'от 91',
                  value: '91'
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
    },
    showSelector() {
      return this.step < 5;
    }
  },
  methods: {
    selectVariant(value) {
      const steps = ['sex', 'age', 'type', 'weight']
      this.selected[steps[this.step - 1]] = value

      if (this.step === 4) {
        axios.get(`${process.env.VUE_APP_API}/fighters`, {
          params: this.selected
        })
          .then(({ data }) => {
            this.fighters = data
          })
      }
      this.step++
    },
    preventSelect(e, step) {
      e.preventDefault();
      this.step = step
    },
  }
}
</script>

<style lang="scss" scoped>
.fighters {
  min-height: 100vh;
  margin-top: 60px;
  padding-top: 60px;
  background: #f9fafd;
  &__selector {
    flex: 1;
    &-wrapper {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      .param {
        margin: 0 15px;
      }
    }
  }

  &__list {
    padding: 30px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-column-gap: 15px;
    grid-row-gap: 15px;
  }

  &__fighter {
  }
}
</style>