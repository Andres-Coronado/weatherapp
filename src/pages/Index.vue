<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input @keyup.enter="getWeatherBySearch" rounded standout v-model="search" label="Search" dark>
        <template v-slot:prepend>
          <q-icon name="my_location" />
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="text = ''" class="cursor-pointer" />
        </template>
      </q-input>
    </div>
    <div></div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div v-if="this.cityDate" class="text-h4 text-weight-light">{{this.cityData.name}}</div>
        <div class="text-h6 text-weight-light">{{this.weatherData.weather[0].main}}</div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{Math.round(this.weatherData.main.temp)}}</span>
          <span class="text-weight-thin text-h3 relative-position degree">
            &deg;C
          </span>
          <div class="col text-center">
            <img
              class="col"
              :src="`https://openweathermap.org/img/wn/${this.weatherData.weather[0].icon}.png`"
              alt=""
              srcset=""
            />
          </div>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="col column text-center text-black">
        <div class="col text-h2 text-weight-thin">Quasar <br />Weather</div>
        <q-btn @click="getLocation()" class="col btn" flat color="white">
          <q-icon left size="3em" name="my_location" />
          <div>Find my location</div>
        </q-btn>
      </div>
    </template>
    <div class="col skyline">
      <img src="" alt="" />
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat:null,
      long:null,
      degres:null,
      degresMax:null,
      degresMin:null,
      cityData:null
    };
  },
  methods:{
    getLocation(){
      navigator.geolocation.getCurrentPosition(position => {
        console.log('position', position)
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords(){
      axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&units=metric&appid=457bfac9db7982bf280b6e53cdf7f39e`)
        .then(response => {
          console.log('response:',response)
          this.weatherData = response.data

        })

    },
    getWeatherBySearch(){

      try{

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=457bfac9db7982bf280b6e53cdf7f39e`)
          .then(response => {
             this.cityData = response.data
                  axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${this.cityData.coord.lat}&lon=${this.cityData.coord.lon}&units=metric&appid=457bfac9db7982bf280b6e53cdf7f39e`)
        .then(response => {
          console.log('response:',response)
          this.weatherData = response.data

        })
          })

      }
      catch{
        console.log("Sth went wrong")
      }

    }
  }
});
</script>

<style lang="sass">
.q-page
  background-color: #11111f
  background-image: linear-gradient(180deg, #11111f 0%, #28b3d6 46%, #ce9329 100%)

.degree
  top: -40px
  right: 18px

.skyline
  flex: 0 0 100px
  background: url("../assets/skyline.png")
  background-size: contain
  background-position: center bottom

.btn
  background-color: #003049
</style>
