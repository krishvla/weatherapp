<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : 'cold' && weather.cod == 404 ? 'error' : 'cold'"
  >
    <main>
      <div class="searchbox">
        <input
          type="text"
          v-model="query"
          @keypress="fetchweather"
          class="searchbar"
          placeholder="Type Your City..."
        />
      </div>
      <div class="weatherwrap" v-if="typeof weather.main != 'undefined'">
        <div class="locationbox">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weatherbox">
          <div class="temp">
            <p>Temperature</p>
            <img src="./assets/weather.svg" alt="Weather logo" width="90px" height="90px" />
            {{ Math.round(weather.main.temp) }}&#176;C
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
          <br />
          <div class="wind">
            <p>Wind Speed</p>
            <img src="./assets/wind.svg" alt="Weather logo" width="90px" height="90px" />
            {{ weather.wind.speed }} m/s
          </div>
        </div>
      </div>
      <div class="weatherwrap" v-if="weather.cod == 404">
        <div class="errorbox">
          <div class="error">
            <p>Unable to Find Your Beautiful Location...</p>
          </div>
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
      api_key: "8303b7fad675015c56455ad3aef295d3",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchweather(e) {
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
      console.log(results);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec"
      ];
      let days = ["Sun", "Mon", "Tue", "Wed", "Thr", "Fri", "Sat"];
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
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("./assets/app.png");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm.svg");
  background-size: cover;
}
#app.cold {
  background-image: url("./assets/snow.svg");
  background-size: cover;
}
#app.error {
  background-image: url("./assets/error.svg");
  background-size: cover;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.35)
  );
}
.searchbox {
  width: 100%;
  margin-bottom: 30px;
}
.searchbox .searchbar {
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
.searchbox .searchbar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.locationbox .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.locationbox .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weatherbox,
.errorbox {
  text-align: center;
}
.weatherbox .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(107, 99, 255, 0.568);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weatherbox .wind {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(107, 99, 255, 0.568);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
p {
  text-align: center;
  color: #fff;
  font-size: 40px;
  font-style: italic;
}
.errorbox .error {
  padding: 10px 25px;
  color: rgb(231, 75, 75);
  font-size: 60px;
  font-weight: 900;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weatherbox .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media only screen and (max-width: 600px) {
  .weatherbox .temp {
    font-size: 60px;
    font-weight: 900;
  }
  .weatherbox .wind {
    font-size: 60px;
    font-weight: 900;
  }
  p {
    text-align: center;
    color: #fff;
    font-size: 30px;
    font-style: italic;
  }
}
</style>
