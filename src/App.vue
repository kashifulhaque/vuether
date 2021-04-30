<template>
  <div
    id="app"
    :class="typeof weatherData.main == 'undefined' ? 'thunder' : weatherData.weather[0].main == 'Clear' ? 'clear' : weatherData.weather[0].main == 'Rain' ? 'rain' : weatherData.weather[0].main == 'Clouds' ? 'clouds' : weatherData.weather[0].main == 'Thunderstorm' ? 'thunder' : weatherData.weather[0].main == 'Haze' ? 'haze' : weatherData.weather[0].main == 'Drizzle' ? 'drizzle' : weatherData.weather[0].main == 'Mist' ? 'mist' : weatherData.weather[0].main == 'Dust' ? 'dust'  : 'mist'"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Enter a city..."
          v-model="query"
          v-on:keypress="fetchWeather"
        />
      </div>

      <div
        class="weather-container"
        v-if="typeof weatherData.main != 'undefined'"
      >
        <div class="location-box">
          <div class="location">
            {{ weatherData.name }}, {{ weatherData.sys.country }}
          </div>
          <div class="humidity">Humidity: {{ weatherData.main.humidity }}%</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weatherData.main.temp) }}°C
            <div class="feels-like">
              Feels like {{ weatherData.main.feels_like }}°C
              <br />
              Min: {{ weatherData.main.temp_min }}°C
              •
              Max: {{ weatherData.main.temp_max }}°C
            </div>
            </div>
          <div class="weather">
            {{ getWeatherStatus(weatherData.weather[0]) }}
          </div>
          <div class="credit">
            Photos from <a href="https://www.reddit.com/r/ImaginarySliceOfLife" target="_blank" rel="noreferrer noopener">r/ImaginarySliceOfLife</a>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import config from "../config.json";

export default {
  name: "App",
  data() {
    return {
      apiUrl: "https://api.openweathermap.org/data/2.5/weather/",
      query: "",
      weatherData: {},
    };
  },
  methods: {
    fetchWeather(event) {
      if (event.key == "Enter" && this.query.length != 0) {
        fetch(
          `${this.apiUrl}?q=${this.query}&units=metric&appid=${config.weatherApiKey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setData)
          .catch((err) => {
            console.error(err.message);
          });
      }
    },
    setData(res) {
      this.weatherData = res;
    },
    getWeatherStatus(city) {
      const status = city.main;
      switch(status.toLowerCase()) {
        case "clear": return "Clear skies ☀";
        case "clouds": return "Clouds... ☁";
        case "rain": return "Rain 🌧";
        case "thunderstorm": return "Thunderstorm ⚡";
        case "haze": return "Haze 🌁";
        case "snow": return "Snow Snow Snow ❄⛄";
        case "mist": return "Mist 🌫";
        case "drizzle": return "Drizzle 🌦";
        case "dust": return "Dust 🌪";
        default: return status;
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Jost&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Jost:wght@800&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@900&display=swap");

*,
html {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  height: 100%;
  min-height: 100%;
}

body {
  margin: 0;
  padding: 0;
  font-family: "Jost", sans-serif;
  background-color: #363636;
}

#app {
  background-image: url("./assets/sunny.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  background-repeat: no-repeat;
  width: 100vw;
  height: 100vh;
  margin: auto;
}

@media only screen and (min-width: 768px) {
  #app {
    width: 30vw;
  }
}

#app.clear {
  background-image: url("./assets/clear.jpg");
}

#app.clouds {
  background-image: url("./assets/cloudy-evening.jpg");
}

#app.rain {
  background-image: url("./assets/rain.jpg");
}

#app.snow {
  background-image: url("./assets/snow.jpg");
}

#app.thunder {
  background-image: url("./assets/thunder.jpg");
}

#app.haze {
  background-image: url('./assets/haze.jpg');
}

#app.mist {
  background-image: url('./assets/mist.jpg');
}

#app.drizzle {
  background-image: url('./assets/drizzle.jpg');
}

#app.dust {
  background-image: url('./assets/dust.jpg');
}

main {
  min-height: 100vh;
  padding: 24px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 32px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 16px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 100px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.4);
  font-family: "Jost", sans-serif;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.4);
  border-radius: 12px;
}

.location-box .location {
  color: #f3f3f3;
  font-size: 36px;
  font-family: "Jost", sans-serif;
  font-weight: 800;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .humidity {
  color: #f3f3f3;
  font-size: 16px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  opacity: 0.8;
  transition: 0.4s;
}

.location-box .humidity:hover {
  font-family: 'Jost', sans-serif;
  font-style: normal;
  font-weight: 800;
  font-size: 18px;
  color: #282828;
  opacity: 1;
  background-color: #eec0c6;
  background-image: linear-gradient(315deg, #eec0c6 0%, #7ee8fa 74%);
  border-radius: 999px;
  padding: 8px;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 12px 24px;
  color: #f3f3f3;
  font-size: 96px;
  font-family: "Montserrat", sans-serif;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 24px 0px;
  transition: 0.4s;
  box-shadow: 3px 6px rgba(255, 0, 0, 0.25);
  display: flex;
  flex-direction: column;
}

.weather-box .temp .feels-like {
  font-family: 'Jost', sans-serif;
  font-size: 20px;
  text-shadow: none;
  color: #ffff00;
}

.weather-box .temp:hover {
  background-color: rgba(24, 24, 24, 0.5);
  border-radius: 8px;
}

.weather-box .weather {
  color: #f3f3f3;
  font-size: 24px;
  font-weight: 600;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .credit {
  font-family: 'Jost', sans-serif;
  background-color: rgba(0, 0, 0, 0.5);
  font-size: 16px;
  padding: 16px;
  border-radius: 16px;
  color: #f3f3f3;
  text-decoration: none;
  margin-top: 16px;
}

.weather-box .credit a {
  color: #ffff00;
  text-decoration: none;
  transition: 0.4s;
}

.weather-box .credit a:hover {
  font-size: 24px;
}
</style>
