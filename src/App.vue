<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 70 ? 'warm' : ''">
    <main>
      <p class="created-by">
        Created by Ian Belfatto, using:
        <a href="http://www.openweathermap.org" style="color:#e1e1e1" target="_blank">OpenWeatherMap</a>
      </p>
      <br />
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>
      <div class="text-center">
        <button type="button" class="btn btn-light btn-sm" v-on:click="fetchWeatherClick()">
          Get Weather
        </button>
      </div>
      <br />
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
          <br />
          <br />
          <div class="tag-titles">Now</div>
          <br />
          <div class="location">{{ message }}</div>
          <br />
          <div class="tag-titles">Conditions</div>
          <br />
          <div class="location">{{ conditions }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "d937f5bdc988400e8f8ba64ae5cf7f2c",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      message: "",
      conditions: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter" && this.query != "") {
        fetch(`${this.url_base}weather?q=${this.query},US&units=imperial&APPID=${this.api_key}`)
          .then(response => {
            return response.json();
          })
          .then(this.setResults);
      }
    },
    fetchWeatherClick() {
      if (this.query != "") {
        fetch(`${this.url_base}weather?q=${this.query},US&units=imperial&APPID=${this.api_key}`)
          .then(response => {
            return response.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      /* Temp Logic */
      if (this.weather.main.temp < 65) {
        console.log("cold");
        this.message = "It's cold, bring a jacket.";
      }
      if (this.weather.main.temp > 65 && this.weather.main.temp < 75) {
        console.log("warm");
        this.message = "It's chilly, just a light jacket today.";
      }
      if (this.weather.main.temp > 75) {
        console.log("hot");
        this.message = "It's hot out, shorts and t-shirt or you'll be all sweaty.";
      }
      /* Conditions Logic */
      if (this.weather.weather[0].main == "Clouds") {
        this.conditions = "It's cloudy af.";
      }
      if (this.weather.weather[0].main == "Clear") {
        this.conditions = "Clear skies, good vibes.";
      }
      if (this.weather.weather[0].main == "Atmosphere") {
        this.conditions = "Hella dangerous out there, don't leave the house.";
      }
      if (this.weather.weather[0].main == "Snow") {
        this.conditions = "It's snowing. Go grab your shovel and dig your car out.";
      }
      if (this.weather.weather[0].main == "Rain") {
        this.conditions = "It's raining, it's pouring, the old man is snoring.";
      }
      if (this.weather.weather[0].main == "Drizzle") {
        this.conditions = "Fo' shizzle my drizzle.";
      }
      if (this.weather.weather[0].main == "Thunderstorm") {
        this.conditions = "*Thunderstruck by Metallica starts playing*";
      }
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${month} ${date} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Inter", sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpeg");
}

main {
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
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

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 620;
  /* text-align: center; */
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.25); */
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  /* text-align: center; */
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.tag-titles {
  color: #e1e1e1;
  font-weight: 600;
}

.created-by {
  color: #e1e1e1;
  font-weight: 500;
  font-size: 12px;
  text-align: center;
}
</style>
