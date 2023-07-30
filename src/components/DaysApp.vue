<template>
  <div class="days-tab">
    <div v-if="loading" class="loading">Loading...</div>
    <ul v-else>
      <li v-for="day in forecast" :key="day.data" class="li_active">
        <div class="day">{{ getDayName(day.date) }}</div>
        <div class="degreece">{{ day.temperature }}&deg;C</div>
        <div class="description">
          <div><img :src="day.iconUrl" /></div>
          <div>{{ day.description }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>



<script>
import axios from 'axios';
import moment from 'moment';

export default {
  props: {
    cityname: String
  },
  data() {
    return {
      forecast: [],
      loading: true,
      iconUrl: null,
    }
  },
  mounted() {
    this.fetchWeatherData();
  },

  methods: {
    async fetchWeatherData() {
      const apiKey = '6d4f8a03f90df1819a1437ce2d790a06';
      const city = this.cityname;
      const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`

      await axios.get(apiUrl).then(Response => {
        console.log(Response, "working")
        const forecastData = Response.data.list;
        const filterData = forecastData.map(item => {
          return {
            date: moment(item.dt_txt.split(' ')[0]),
            temperature: Math.round(item.main.temp),
            description: item.weather[0].description,
            iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`
          };
        }).reduce((acc, item) => {
          if (!acc.some(day => day.date.isSame(item.date, 'day'))) {
            acc.push(item);
          }
          return acc;
        }, []).slice(1, 6);
        console.log(filterData, "working")
        this.forecast = filterData;
        this.loading = false;
      }).catch(error => {
        console.error('Error fatching weather data: ', error);
        this.loading = false;
      });
    },
    getDayName(date) {
      return date.format('ddd');
    }
  }
}
</script>



<style></style>