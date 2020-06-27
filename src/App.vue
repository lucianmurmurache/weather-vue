<template>
  <div
    id="app"
    :class="
    typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' || 
    typeof weather.main != 'undefined' && weather.main.temp < 0 ? 'frozen' : ''"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search.."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather-status">{{weather.weather[0].main}}</div>
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
      api_key: "9b607b30291273bced1d520cf8e767a4",
      url_base: "https://api.openweathermap.org/data/2.5/", //https://openweathermap.org/appid
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
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
        "December"
      ];
      let days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Roboto", sans-serif;
}

#app {
  background: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.5s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

#app.frozen {
  background-image: url("./assets/frozen-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 2rem;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

/*=== Seach box and search bar ===*/
.search-box {
  width: 100%;
  margin-bottom: 2rem;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 1rem;
  color: #363636ab;
  font-size: 1.5rem;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 1rem 0 1rem 0;
  box-shadow: 0px 0px 0.5rem rgba(0, 0, 0, 0.4);
  transition: 0.5s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 1rem rgba(0, 0, 0, 0.5);
  border-radius: 0 1rem 0 1rem;
}

/*=== Location ===*/
.location-box .location {
  color: #fff;
  font-size: 2.5rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

/*=== Date ===*/
.location-box .date {
  color: #fff;
  font-size: 1.5rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

/*=== Weather box===*/
.weather-box {
  text-align: center;
}
/*=== Temperature ===*/
.weather-box .temperature {
  display: inline-block;
  padding: 0.8rem 1.8rem;
  font-size: 5rem;
  font-weight: 700;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 1rem;
  margin: 1.875rem 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

/*=== Weather status ===*/
.weather-box .weather-status {
  color: #fff;
  font-size: 3rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
