<template>
  <div id="app">
   
    <div class="wrapper">
      <div class="title">Hava Durumu</div> <br>
      <div class="inputButtonWrapper">
        <input v-model="typedCity" class="chosingCity" type="text" placeholder="Bir şehir ismi girin">
        <button :disabled="typedCity==''" @click="getCity" type="button" class="searchButton">
          Ara
        </button>
      </div>
      <div v-if="errorHandler" class="error">
        Hatalı bir arama yaptınız. Lütfen geçerli bir şehir giriniz.
      </div>
      <div v-if="loading" class="error">
        Yükleniyor...
      </div>
      <div v-if="weatherData.name">
      <div class="chosedCity">
        {{weatherData.name}}
      </div>
      <div :key="key" v-for="(item,key) in weatherData.weather" class="weatherStyle">
         {{item.description}}
      </div>
      <div class="temperature">
         {{temperatureManupulator+'°'}}
      </div>
      <div class="minMaxWrapper">
      <div>
         min <br> 
         <span class="minMaxTemperature">
          {{weatherData.main.temp_min+'°'}}
        </span>
      </div>
          <div class="minMax"> maks <br> 
            <span class="minMaxTemperature">
              {{weatherData.main.temp_max+'°'}}
            </span> 
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: 'App',
    data() {
      return {
        typedCity: '',
        weatherData: {},
        errorHandler: false,
        loading: false
      }
    },
    methods: {
      getCity() {
        this.weatherData = {}
        this.loading = true
        this.errorHandler = false
        const apiURL =
          `https://api.openweathermap.org/data/2.5/weather?q=${this.typedCity}&units=metric&lang=tr&appid=984aef41b67c466dec43cc39da136721`
        axios.get(apiURL).then(response => {
          this.weatherData = response.data
          this.loading = false
        }).catch(error => {
          this.errorHandler = true
          this.loading = false
        })
      }
    },
    computed: {
      temperatureManupulator() {
        return this.weatherData.main.temp.toFixed(1)
      }
    },
  }
</script>

<style>
  #app {
    display: flex;
    justify-content: space-around;
  }
  body {
    background-color: #5193F1;
  }
  .wrapper {
    background-color: #67A9F3;
    text-align: center;
    width: 555px;
    padding: 20px 15px 20px 15px;
    border-radius: 15px;
    margin-top: 70px;
  }
  .chosingCity,
  ::placeholder {
    font-size: 25px;
    width: 100%;
    height: 55px;
    border-right-style: none;
    border-left-style: none;
    border-top-style: none;
    border-bottom-color: #d6dfe796;
    background: #67A9F3;
    color: #d6dfe7d5;
    padding-left: 10px;
  }
  .searchButton {
    width: 110px;
    height: 55px;
    font-size: 20px;
    font-weight: bold;
    background: #FEFEFE;
    color: #5884F0;
    border-style: none;
    border-radius: 5px;
    margin-left: 15px;
    font-family: Arial, Helvetica, sans-serif;
  }
  input:focus {
    outline: none;
  }
  .chosedCity {
    text-transform: uppercase;
    text-align: center;
    font-size: 30px;
    color: #fefefed2;
    margin-bottom: 40px;
    margin-top: 70px;
    letter-spacing: 5px;
    font-weight: 700;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  .searchButton:active {
    color: white;
    background-color: #67A9F3;
  }
  .weatherStyle {
    text-transform: uppercase;
    font-size: 25px;
    color: #fefefeb9;
    text-align: center;
    margin-top: 15px;
  }
  .temperature {
    text-align: center;
    font-size: 80px;
    height: 50px;
    margin-top: 70px;
    color: white;
    font-weight: bold;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  .minMax {
    border-left: 1px solid #fafafb;
    margin-left: 15px;
    padding-left: 15px;

  }
  .minMaxWrapper {
    color: white;
    font-size: 25px;
    display: flex;
    justify-content: center;
    margin-top: 70px;
    font-weight: bold;
  }
  .minMaxTemperature {
    background-color: #67A9F3;
    font-weight: 100 !important;
  }
  .error {
    font-size: 20px;
    background-color: white;
    color: #67A9F3;
    margin-top: 20px;
    border-radius: 5px;
    padding: 5px;
  }
  .searchButton:disabled {
    color: white;
    background-color: rgb(54, 54, 234);
  }
  .inputButtonWrapper {
    display: flex;
  }
  .title{
    font-size: 40px;
    color: rgba(255, 255, 255, 0.803);

  }
</style>