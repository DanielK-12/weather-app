<template>
  <div id="app" :class="(typeof weather.main !== 'undefined' && weather.main.temp < 16 ? 'cold' : '')">
    <main>
      <div class="search-box">
        <input name="" id="" type="text" class="search-bar" v-model="query" @keypress="fetch_weather">
      </div>
      <div class="weather-wrap" v-if="(typeof weather.main != 'undefined')">
        <div class="location-box">
          <div class="location">{{ weather.name }} {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather-image"><img id="wicon" alt="Weather icon"
              v-bind:src="`http://openweathermap.org/img/w/${weather.weather[0].icon}.png`"></div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="weather-description">{{ weather.weather[0].description }}</div>
        </div>
        <DaysApp :cityname="query"></DaysApp>
      </div>
      <div>
      </div>
    </main>
  </div>
</template>

<script>
import DaysApp from './components/DaysApp.vue'

export default {
  name: 'App',
  components: {
    DaysApp
  },
  props: {
    city: String,
  },
  data() {
    return {
      cityname: this.city,
      api_key: 'e52ffea374af185784c3095c2523eae4',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetch_weather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'poppins', sans-serif;
}

#app {
  background-image: url("assets/hot-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.cold {
  background-image: url("assets/cold-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  margin: 0 auto;
  width: 50%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: 313131;
  font-size: 20px;
  box-shadow: 0px, 0px, 16px rgba(0, 0, 0, 0.25);

  appearance: none;
  border: none;
  background: none;
  outline: none;

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba (255, 255, 255, 0.9);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 600;

  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  margin: 30px 0px;
  box-shadow: 3px 3px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 30px;
  font-weight: 300;
}

.weather-box .weather-description {
  color: #fff;
  font-size: 15px;
  font-weight: 300;
}

.weather-box .weather-image {
  position: center;
}
</style>
