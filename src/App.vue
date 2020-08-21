<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search City" v-model="query" @keypress="fetchWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].description }}</div>
          <Skycons v-bind:icon="icon" v-bind:url_base_icon="url_base_icon" v-bind:end="end" :cords_id="cords_id" v-bind:api_key="api_key"/>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Skycons from './components/Skycons.vue'

export default {
  name: 'App',
  components: {
    Skycons
  },
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {},
      url_base_icon: "http://openweathermap.org/img/wn/",
      icon: "",
      end: "@2x.png",
      cords_id: ''
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      console.log(results);
      this.weather = results;
      // this.icon = results.weather[0].icon;
      this.cords_id = results.id;
    },
    dateBuilder() {
      let d = new Date();
      // let f = new Date().toLocaleString("en-US", {timeZone: "Asia/Jakarta"});
      // console.log(f);
      
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "Septemper", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];


      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
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
  font-family: Arial, Helvetica, sans-serif;
  width: 100vw;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-position: center; 
  background-repeat: no-repeat; 
  background-size: cover;
  transition: .5s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  background-position: center; 
  background-repeat: no-repeat; 
  background-size: cover;
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgab(0, 0, 0, .25), rgba(0, 0, 0, .75));
}

.search-box {
  width: 30%;
  margin: 0 auto 30px auto;
}

.search-box input {
  text-align: center;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .5);
  border-radius: 0px 16px 0px 16px;
  transition: .5s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather {
  color: #fff;
  font-size: 47px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}

@media screen and (max-width: 750px) {
  .search-box {
    width: 60%;
    margin: 0 auto 30px auto;
  }
}
</style>
