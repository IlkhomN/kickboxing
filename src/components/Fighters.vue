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
                        title: 'Children (7-9)',
                        value: 'children'
                        },                        {
                        title: 'Younger cadets (10-12)',
                        value: 'younger-cadets'
                        },
                        {
                        title: 'Seniors (18-45)',
                        value: 'seniors'
                        },
                    ]
                    }
                } else if (this.step === 3) {
                    if (['children'].includes(this.selected.age)) {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Kick Light',
                            value: 'kick-light'
                        },
                        {
                            title: 'Light contact',
                            value: 'light-contact'
                        },
                        ]
                    }
                    } else if (['younger-cadets'].includes(this.selected.age)) {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Kick Light',
                            value: 'kick-light'
                        },
                        {
                            title: 'Light contact',
                            value: 'light-contact'
                        },
                        ]
                    }
                    } else {
                    return {
                        title: 'Направление',
                        variants: [
                        {
                            title: 'Full contact',
                            value: 'full-contact'
                        },
                        {
                            title: 'K-1',
                            value: 'k-1'
                        },
                        ]
                    }
                    }
                } else if (this.step === 4) {
                    if (this.selected.age === 'children') {
                    if (this.selected.sex === 'male') {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 18',
                            value: '-18'
                            },{
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
                            title: 'до 18',
                            value: '-18'
                            },{
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
                    }
                    } else if (this.selected.age === 'younger-cadets') {
                        if (this.selected.sex === 'male') {
                            return {
                            title: 'Весовая категория',
                            variants: [
                                {
                                title: 'до 28',
                                value: '-28'
                                },{
                                title: 'до 32',
                                value: '-32'
                                },
                                {
                                title: 'до 37',
                                value: '-37'
                                },
                                {
                                title: 'до 42',
                                value: '-42'
                                },
                                {
                                title: 'до 47',
                                value: '-47'
                                },
                                {
                                title: 'от 47',
                                value: '47'
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
                                },{
                                title: 'до 32',
                                value: '-32'
                                },
                                {
                                title: 'до 37',
                                value: '-37'
                                },
                                {
                                title: 'до 42',
                                value: '-42'
                                },
                                {
                                title: 'до 47',
                                value: '-47'
                                },
                                {
                                title: 'от 47',
                                value: '47'
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
                            title: 'до 52.7',
                            value: '-52.7'
                            },{
                            title: 'до 56.4',
                            value: '-56.4'
                            },
                            {
                            title: 'до 60',
                            value: '-60'
                            },
                            {
                            title: 'до 64.5',
                            value: '-64.5'
                            },
                            {
                            title: 'до 66.8',
                            value: '-66.8'
                            },
                            {
                            title: 'до 71.8',
                            value: '-71.8'
                            },
                            {
                            title: 'до 75',
                            value: '-75'
                            },
                            {
                            title: 'до 81.4',
                            value: '-81.4'
                            },
                            {
                            title: 'до 85',
                            value: '-85'
                            },
                            {
                            title: 'до 88.6',
                            value: '-88.6'
                            },
                            {
                            title: 'до 94.1',
                            value: '-94.1'
                            },
                            {
                            title: 'от 94.1',
                            value: '94.1'
                            },
                        ]
                        }
                    } else {
                        return {
                        title: 'Весовая категория',
                        variants: [
                            {
                            title: 'до 55',
                            value: '-55'
                            },
                            {
                            title: 'до 63',
                            value: '-63'
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