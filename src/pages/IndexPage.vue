<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        @keyup.enter="getWeatherBySearch"
        placeholder="Search"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon name="my_location" />
        </template>
        <template v-slot:append>
          <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{ weatherData.name }}</div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img :src="'https://openweathermap.org/img/wn/10d@2x.png'" />
      </div>
      <span class="text-h4 relative-position text-center">{{}}</span>
    </template>

    <template v-else>
      <div class="mid col column text-center text-white">
        <div class="col text-h2 text-weight-thin">Weather App</div>
        <q-btn @click="getLocation" class="col" flat>
          <q-icon left size="3em" name="my_location" />
          <div class="">Lokasyon Seçin</div>
        </q-btn>
      </div>
    </template>

    <div class="col skyline"></div>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  name: "IndexPage",

  data() {
    return {
      search: "",
      weatherData: null,
      lat: "",
      lon: "",
      icon: "",
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: "096ff5f9edc547219d152212a709390c",
    };
  },

  //navigator.geolocation.getCurrentPosition(showPosition)  bu bir WebApi ,APİ geolocation
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        console.log("pozisyon:", position);
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;

        console.log(this.lat, this.lon);
        this.getWeatherByCoords();
      });
    },

    getWeatherByCoords() {
      axios
        .get(
          "http://api.openweathermap.org/data/2.5/weather?lat={lat}lon={lon}&q=Ankara,tr&APPID=096ff5f9edc547219d152212a709390c&units=metric"
        )
        .then((response) => {
          this.weatherData = response.data;
          console.log("obje geldi" + this.weatherData);
          console.log("api key" + this.apiKey);
        })
        .catch((error) => console.log(error));
    },

    getWeatherBySearch() {
      console.log("search calisti");
      axios
        .get(
          "http://api.openweathermap.org/data/2.5/weather?q=Paris,fr&appid=096ff5f9edc547219d152212a709390c&units=metric"
        )
        .then((response) => {
          this.weatherData = response.data;
        })
        .catch((error) => console.log("hata geldi " + error));
    },
  },
};
</script>
<style scoped></style>
