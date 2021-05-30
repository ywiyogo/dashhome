<template>
  <q-page
    class="flex q-pa justify-center"
    :class="backgrndClass"
    style="height: 100vh; width: 100%"
  >
    <div
      class="row full-width mx-auto q-pt-md text-h2 text-white text-center block"
    >
      Weather Dashboard
    </div>
    <div class="row full-width justify-center" style="height: 75%">
      <div class="column q-mr-lg q-mb-xl justify-center">
        <div class="col justify-center" style="max-height: 70px">
          <q-input
            v-model="weatherData.cityName"
            label="Search city"
            @keyup.enter="getWeatherBySearch"
            placeholder="Search"
            dark
            borderless
            class=""
          >
            <template v-slot:before>
              <q-icon name="my_location" />
            </template>

            <template v-slot:append>
              <q-icon
                v-if="weatherData.cityName !== ''"
                name="close"
                @click="clearData($event)"
                class="cursor-pointer"
              />
              <q-icon name="search" />
            </template>
          </q-input>
        </div>
        <div class="col justify-center">
          <div class="text-h1 text-white text-weight">
            {{ time }}
          </div>
        </div>
        <div class="col justify-center text-h2 text-grey-2 q-mx-auto">
          {{ weatherData.cityName }}
        </div>
        <template v-if="!weatherData.temp">
          <div class="col justify-center text-h2">
            <q-btn
              class="q-mx-auto block justify-center"
              @click="getLocation"
              flat
            >
              <q-icon left name="my_location" />
              <div class="text-center">Find my location</div>
            </q-btn>
          </div>
        </template>
        <template v-else>
          <div
            class="col justify-center q-my-auto q-mx-auto text-h3 text-weight-light text-white"
          >
            <div class="row">
              <div class="col q-my-auto">{{ weatherData.main }}</div>
              <div class="col"><img
              :src="`http://openweathermap.org/img/wn/${weatherData.icon}@2x.png`"
            /></div>
            </div>
          </div>
          <div
            class="col justify-center q-mx-auto text-h1 text-weight text-white"
          >
            {{ Math.round(weatherData.temp) }}° C
          </div>
        </template>
      </div>
      <template v-if="weatherData.temp">
        <div class="column justify-between" style="width: 65%">
          <div class="col text-white text-center">
            <Highlights
              :p_humidity="weatherData.humidity"
              :p_uvi="weatherData.uvi"
              :p_airquality="40"
              :p_wind="weatherData.wind_speed"
            ></Highlights>
            <div class="col q-mr-xl" style="height=500px;">
              <EchartLine
                :p_hourly="weatherData.hourly"
                :p_daily="weatherData.daily"
                :p_sun="weatherData.sunny"
              ></EchartLine>
            </div>
          </div>
        </div>
      </template>
    </div>
    <div class="row full-width skyline"></div>
  </q-page>
</template>

<script>
import Highlights from "components/Highlights.vue";
import ApexchartLine from "components/ApexchartLine.vue";
import EchartLine from "components/EchartLine.vue";

export default {
  name: "PageIndex",
  components: {
    Highlights,
    ApexchartLine,
    EchartLine,
  },
  data() {
    return {
      search: "",
      localTime: "",
      interval: null,
      dataInterval: null,
      time: null,
      weatherData: {
        cityName: "",
        temp: null,
        icon: "",
        main: "",
        humidity: null,
        rainfall: null,
        windSpeed: null,
        windDegree: null,
        pressure: null,
        sunny: null,
        uvi: null,
        visibility: null,
        hourly: null,
        daily: null,
      },
      lat: null,
      lon: null,
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      //https://api.openweathermap.org/data/2.5/onecall?lat={lat}&lon={lon}&exclude={part}&appid={API key}
      oneCallApiUrl: "https://api.openweathermap.org/data/2.5/onecall",
      apikey: null,
      excludeData: "minutely",
    };
  },
  methods: {
    async getLocation() {
      try {
        this.$q.loading.show(); //add loading animation, depends on Loading plugins

        if (this.$q.platform.is.electron) {
          let response = await this.$axios.get("https://freegeoip.app/json/");
          console.log(response.data);
          this.lat = response.data.latitude;
          this.lon = response.data.longitude;
          this.getWeatherByCoords();
        } else {
          navigator.geolocation.getCurrentPosition((position) => {
            this.lat = position.coords.latitude;
            this.lon = position.coords.longitude;
            this.getWeatherByCoords();
          });
        }
      } catch (e) {
        console.error(e);
        let alertmsg =
          e +
          "\nIf you didn't have an API key, plese create it on OpenWeatherMap site.";
        alert(alertmsg);
      }
    },
    async getWeatherByCoords() {
      try {
        this.$q.loading.show();
        let response = await this.$axios(
          // `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${process.env.API_KEY_WEATHER}&units=metric`
          `${this.oneCallApiUrl}?lat=${this.lat}&lon=${this.lon}&exclude=${this.excludeData}&appid=${process.env.API_KEY_WEATHER}&units=metric`
        );
        this.fillWeatherData(response.data, true);
        response = await this.$axios(
          `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${process.env.API_KEY_WEATHER}&units=metric`
        );
        this.weatherData.cityName = response.data.name;
      } catch (e) {
        console.error(e);
        let alertmsg =
          e +
          "\nIf you didn't have an API key, plese create it on OpenWeatherMap site and place it in the .env file.";
        alert(alertmsg);
      }
      this.$q.loading.hide();
      let _this = this;
      if (!_this.dataInterval) {
        _this.dataInterval = setInterval(function () {
          _this.getWeatherByCoords();
        }, process.env.UPDATE_IN_MS); //update in ms from the .env file
      }
      this.$q.loading.hide();
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?q=${this.weatherData.cityName}&appid=${process.env.API_KEY_WEATHER}&units=metric`
      ).then((response) => {
        // console.log("response by search: ", response);
        this.fillWeatherData(response.data, false);
        this.$q.loading.hide();
      });
      if (!_this.dataInterval) {
        _this.dataInterval = setInterval(function () {
          _this.getWeatherByCoords();
        }, process.env.UPDATE_IN_MS); //update in ms from the .env file
      }
    },

    fillWeatherData(responseData, isOneCall) {
      if (isOneCall) {
        this.weatherData.temp = responseData.current.temp;
        this.weatherData.icon = responseData.current.weather[0].icon;
        this.weatherData.main = responseData.current.weather[0].main;
        this.weatherData.humidity = responseData.current.humidity;
        this.weatherData.windSpeed = responseData.current.wind_speed;
        this.weatherData.windDegree = responseData.current.wind_deg;
        this.weatherData.pressure = responseData.current.pressure;
        this.weatherData.sunny = [
          responseData.current.sunrise,
          responseData.current.sunset,
        ];
        this.weatherData.uvi = responseData.current.uvi;
        this.weatherData.visibility = responseData.current.visibility;
        this.weatherData.hourly = responseData.hourly;
        this.weatherData.daily = responseData.daily;
      } else {
        this.weatherData.cityName = responseData.name;
        this.weatherData.temp = responseData.main.temp;
        this.weatherData.icon = responseData.weather[0].icon;
        this.weatherData.main = responseData.weather[0].main;
        this.weatherData.humidity = responseData.main.humidity;
        this.weatherData.windSpeed = responseData.wind.speed;
        this.weatherData.windDegree = responseData.wind.deg;
        this.weatherData.pressure = responseData.main.pressure;
        this.weatherData.sunny = [
          responseData.sys.sunrise,
          responseData.sys.sunset,
        ];
        this.weatherData.uvi = null;
      }
    },
    clearData(event) {
      this.weatherData = {
        cityName: "",
        temp: null,
        icon: "",
        main: "",
        humidity: null,
        windSpeed: null,
        windDegree: null,
        pressure: null,
        sunrise: null,
        sunset: null,
        uvi: null,
        visibility: null,
        rainfall: null,
      };
    },
  },
  computed: {
    backgrndClass() {
      if (this.weatherData) {
        if (this.weatherData.icon.endsWith("n")) {
          return "backgrnd-night";
        } else {
          return "backgrnd-day";
        }
      }
    },
    getWeatherData() {
      this.FullName;
      return this.FullName !== null && this.FullName !== "";
    },
  },
  beforeDestroy() {
    // prevent memory leak
    clearInterval(this.interval);
  },
  created() {
    // update the time every second
    this.interval = setInterval(() => {
      // Concise way to format time according to system locale.
      // In my case this returns "3:48:00 am"
      this.time = Intl.DateTimeFormat(navigator.language, {
        hour: "numeric",
        minute: "numeric",
        second: "numeric",
        hour12: false,
      }).format();
    }, 1000);
  },
};
</script>

<style lang="sass">
.q-page
  max-height: 100vh
  background: linear-gradient(to left, #fc5c7d, #6a82fb) /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  &.backgrnd-night
    background: linear-gradient(to bottom, #232527, #414346)
  &.backgrnd-day
    background: linear-gradient(to bottom, #00b09b, #96c93d)
.skyline
  flex: 1 0 150px
  background: url(../assets/skyline-2730410_1280_pixabay.png)
  background-size: contain
  background-position: center bottom
.container
  justify-content: space-around
</style>