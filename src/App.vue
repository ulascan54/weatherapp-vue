<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-5" style="max-width:400px">
      <h1 class="title text-center">Hava Durumu</h1>
      <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="Hangi Şehir?"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>

      <p class="text-center my-3" v-if="cityFound">Hiç şehir bulunamadı</p>
      <div
        class="card  my-3 shadow-lg back-card overflow-hidden"
        v-if="visible"
      >
        <div icon="sunny" v-if="clearSky">
          <span class="sun"></span>
        </div>

        <div icon="snowy" v-if="snowy">
          <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
          </ul>
        </div>
        <div icon="stormy" v-if="stormy">
          <span class="cloud"></span>
          <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
          </ul>
        </div>
        <div icon="cloudy" v-if="cloudy">
          <span class="cloud"></span>
          <span class="cloud"></span>
        </div>
        <div icon="nightmoon" v-if="clearNight">
          <span class="moon"></span>
          <span class="cloud"></span>
          <span class="cloud"></span>
        </div>

        <!-- Top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
            <span>...</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>
        <!-- top of card ends here -->

        <!--card middle body, card body used cos I want to just update the innerHTML -->
        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="" alt="" />
                  {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  <img src="" alt="" />
                  {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
          </div>
          <!-- card middle ends here -->

          <!-- card bottom starts here -->
          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>Hissedilen</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}%</p>
              <span>Nem</span>
            </div>
          </div>

          <!-- card bottom ends here -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,
      isDay: true,
      citySearch: "",
      weather: {
        cityName: "İstanbul",
        country: "TR",
        temperature: 12,
        description: "Bulutlar her yerde",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
      },
    };
  },
  methods: {
    getWeather: async function() {
      console.log(this.citySearch);
      const key = "699440d767dae9efbea4cb119468ed1b";
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      try {
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);

        const timeOfDay = data.weather[0].icon;
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
        const mainWeather = data.weather[0].main;
        if (mainWeather.includes("Clouds")) {
          this.cloudy = true;
          this.stormy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Thunderstorm")) {
          this.stormy = true;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clear") && this.isDay) {
          this.clearSky = true;
          this.stormy = false;
          this.cloudy = false;
          this.clearNight = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds") && !this.isDay) {
          this.clearNight = true;
          this.clearSky = false;
          this.stormy = false;
          this.cloudy = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Snow")) {
          this.snowy = true;
          this.stormy = false;
          this.cloudy = false;
          this.clearSky = false;
          this.clearNight = false;
        }
        this.visible = true;
        this.cityFound = false;
      } catch (error) {
        console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    },
  },
};
</script>

<style>
@import "./assets/custom.css";
@import "./assets/animation.css";
</style>
