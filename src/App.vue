<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search Weather"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-display">
        <div class="left">
          <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="weather-box">
              <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            </div>
            <div class="location-box">
              <div class="location">
                {{ weather.name }}, {{ weather.sys.country }}
              </div>
              <div class="date">
                {{ dateBuilder() }}
              </div>
            </div>
          </div>
        </div>
        <div class="right" v-if="typeof weather.main != 'undefined'">
          <div class="weather-type">
            <img :src="icon" />
            <div class="weather-description">
              <h2>{{ weather.weather[0].description }}</h2>
            </div>
          </div>
          <div class="weather-info">
            <div class="max-temp card">
              <h2>Max Temp</h2>
              <span>{{ Math.round(weather.main.temp_max) }}°C</span>
            </div>
            <div class="min-temp card">
              <h2>Min Temp</h2>
              <span>{{ Math.round(weather.main.temp_min) }}°C </span>
            </div>
            <div class="sunrise card">
              <h2>Sun Rise</h2>
              <span>{{ extractTime(weather.sys.sunrise) }}</span>
            </div>
            <div class="sunset card">
              <h2>Sunset</h2>
              <span>{{ extractTime(weather.sys.sunset) }}</span>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name: "app",
    data() {
      return {
        api_key: "ba5c805cc436be9e621d67e231717157",
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: "",
        weather: {},
        icon: ""
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
        console.log("results >>>>>>>>>>>>>>", results);
        this.weather = results;
        this.icon = ` https://openweathermap.org/img/wn/${this.weather.weather[0]["icon"]}@4x.png`;
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
          "Sunday",
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday"
        ];
        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month}, ${year}`;
      },
      extractTime(time) {
        let date = new Date(time * 1000);
        let h = date.getHours();
        let m = "0" + date.getMinutes();
        let t = h + ":" + m.substr(-2);
        return t;
      },
      getsrc() {
        const icon = `https://openweathermap.org/img/wn/${this.weather[0]["icon"]}@2x.png`;
        return icon;
      }
    }
  };
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100;500&display=swap");
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font-family: "Montserrat", sans-serif;
  }

  #app {
    background-image: url(./assets/cold.jpeg);
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app .warm {
    background-image: url(./assets/warm.jpeg);
  }
  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.75)
    );
  }
  .search-box {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 30px;
  }
  .search-box .search-bar {
    display: block;
    width: 50%;
    padding: 15px;
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
    color: #313131;
    font-size: 20px;
    border: none;
    outline: none;
    appearance: none;
    background: none;
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.45);
  }

  .search-box .search-bar:focus {
    background-color: rgba(255, 255, 255, 0.75);
    box-shadow: rgba(0, 0, 0, 0.25);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: left;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: #fff;
    font-size: 20px;
    font-weight: 200;
    text-align: center;
    font-style: italic;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    padding: 10px 25px;
    display: inline-block;
    color: #fff;
    font-weight: 900;
    font-size: 102px;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    /* background-color: rgba(255, 255, 255, 0.25); */
    border-radius: 16px;
    margin: 30px 0px;
    /* box-shadow: 3px 6px rgba(0, 0, 0, 0.25); */
  }

  .weather-box .weather {
    font-size: 48px;
    color: #fff;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-display {
    display: flex;
  }
  .weather-wrap {
    align-items: center;
    display: flex;
    /* flex-direction: column; */
    /* align-items: center; */
    /* margin: auto; */
    /* width: 100%; */
    /* justify-content: center; */
  }
  .left {
    flex: 0.5;

    display: flex;
    /* justify-content: center; */
    align-items: flex-end;
    height: 80vh;
  }
  .right {
    flex: 0.5;
    background-color: rgba(255, 255, 255, 0.15);
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    border-radius: 3px;
    display: flex;
    flex-direction: column;
  }
  .weather-type {
    flex: 0.3;
  }
  .weather-info {
    flex: 0.7;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    grid-gap: 2px;
  }
  .weather-type {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .weather-description > h2 {
    transform: scale(1.2);
    color: #fff;
    font-weight: 800;
  }
  .card {
    border-radius: 3px;
    background-color: rgba(0, 0, 0, 0.25);
    box-shadow: 3px 6px rgba(255, 255, 255, 0.05);
    margin: 5px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .card > h2 {
    color: #fff;
    font-weight: 200;
    flex: 0.4;
  }
  .card > span {
    flex: 0.6;
    font-weight: 800;
    color: #fff;
    font-size: 60px;
  }
  @media only screen and (max-width: 600px) {
    .search-box .search-bar {
      width: 100%;
    }
    .weather-box .temp {
      font-size: 35px;
    }

    .weather-display {
      display: flex;
      flex-direction: column;
    }
    .location-box .location {
      font-size: 25px;
    }
    .location-box .date {
      font-size: 12px;
      text-align: left;
    }
    .weather-description > h2 {
      font-size: 15px;
    }
    .card > h2 {
      font-size: 15px;
    }
    .card > span {
      font-size: 30px;
    }
    .weather-type > img {
      height: 100px;

      object-fit: contain;
    }
  }
  @media only screen and (max-width: 1000px) {
    .location-box .date {
      font-size: 12px;
      text-align: left;
    }
    .weather-box .temp {
      font-size: 40px;
    }
  }
</style>
