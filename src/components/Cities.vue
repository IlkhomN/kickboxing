<template>
  <div class="cities">
    <h2 class="cities__title">Бойцы по городам:</h2>
    <div class="cities__wrapper">
      <div
        v-for="(value, key) in cities"
        :key="key"
        class="cities__city"
      >
        {{ key }}: {{ value }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      cities: {}
    }
  },
  mounted() {
    axios.get(`${process.env.VUE_APP_API}/fighters/cities`)
      .then((result) => {
        this.cities = result.data
      })
  }
}
</script>

<style lang="scss" scoped>
.cities {
  margin-top: 60px;
  padding-top: 60px;
  min-height: 100vh;
  background: #f9fafd;
  &__wrapper {
    padding: 30px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-column-gap: 15px;
    grid-row-gap: 15px;  
  }
  &__title {
    color: #000;
  }
}
</style>