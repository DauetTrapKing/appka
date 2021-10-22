<template >
<v-navigation-drawer 
        absolute
        right
      >
          <v-card>
            <v-img 
              :src="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'srcWarm' : ''"
            > 
              <div 
                v-if="loading"
                class="d-flex justify-content-center align-items-center p-3"
              >
              <v-progress-circular
                class="mx-auto mt-4"
                justify-center
                indeterminate color="white"
                >
              </v-progress-circular>
              </div>
              <v-row>
                <v-col class="ml-3">
                  <v-text-field
                    v-model="query"
                    v-if="!loading"
                    label="City"
                    @keypress='fetchWeather'
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <template v-if="weather.main">
                <v-card-text class="text-center font-color-white text-h4">{{ weather.name }} {{ weather.sys.country }}</v-card-text>
                <v-card-text class="text-center">{{ dateBuilder() }}</v-card-text>
                <v-card-text class="text-center  text-h4">{{weather.weather[0].main}}</v-card-text>
                <v-card-text class="text-center font-weight-black text-h2">{{Math.round(weather.main.temp)}}°C</v-card-text>
              </template>
              <templete v-else dark>
                <v-card-text class="text-center font-weight-black text-h4" >{{weather.message}}</v-card-text>
              </templete>
            </v-img>
          </v-card> 
      </v-navigation-drawer>
</template>
<script>
export default({
    methods:{
    async fetchWeather (e) {
      if (e.key == "Enter") {
          this.loading=!false;
          await new Promise((resolve) => {
          setTimeout(() => {
            resolve();
            this.loading=!true;
          }, 2000);
        });
        let res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        this.weather = await res.json();
      }
    },
    setResults (results) {
      this.weather = results;
    },     
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    },
    data () {
    return {
      api_key: '328cf28a4df46acf9e9d7c39ce5fd1f1',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      loading: false,
      src: require('@/assets/cold.jpg'),
      srcWarm: require("@/assets/warm.jpg")
    }
  },
})
</script>
