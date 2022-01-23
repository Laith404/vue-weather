<template>
  <div 
  id="app" 
  :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="search..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp)}}°c</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '0e87ce0c4d204cea8d47b1edbb415315',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults)
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
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
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;400;500;600;900&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{ 
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image: url('assets/bg.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
.warm {
  background-image: url('assets/bg2.jpg')!important;
}
main {
  min-height: 100vh;
  padding: 1.5625rem;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,.75))
}

.search {
  width: 100%;
  margin-bottom: 1.875rem;
}

.search .search-bar {
  display: block;
  width: 100%;
  padding: .9375rem;
  color: #313131;
  font-size: 1.25rem;

  appearance: none;
  border: none;
  outline: none;

  box-shadow: 0 0 .5rem rgba(0,0,0, .25);
  background-color: rgba(255,255,255,.5);
  border-radius: 0 16px;
  transition: .4s;
}

.search .search-bar:focus {
  background-color: rgba(255,255,255,.75);
  box-shadow: 0 0 1rem rgba(0,0,0, .25);
  border-radius: 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,.25);
}

.location-box .date {
  color: #fff;
  font-size: 1.375rem;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: .625rem 1.85rem;
  color: #fff;
  font-size: 6.375rem;
  font-weight: 900;
  
  text-shadow: .1875rem .375rem rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.25);
  border-radius: .9375rem;
  margin: 1.875rem 0;
  
  box-shadow: .1875rem .375rem rgba(0,0,0,0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 3rem;
  font-weight: 600;
  font-style: italic;
  text-shadow: .1875rem .375rem rgba(0,0,0,0.25);
}
</style>
