<template>
  <div id="app" 
      v-bind:class="className">
      <main >
        <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
        </div>
        <div v-if="Object.keys(this.weather).length < 1" class="enterLocation">
          <h2>Enter the location</h2>
        </div>

        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="hasShowMoreButton">
        <button class="showMoreButton" 
        v-on:click="doActions()" 
        >{{ buttonText }}</button>
      </div>
      <div v-if="Object.keys(this.weather).length > 1 && this.view" class="showMore" :class="(this.view) ? 'show': 'hide'">
        <show-more :weather="this.weather"></show-more>
      </div>
      
      <div v-if="this.enterValidLocation" class="enterLocation" >
        <h2>Please Enter Valid Location</h2>
      </div>
      
    </main>
  </div>
</template>

<script>
import showMore from './components/showMore.vue';

export default {
  name: 'App',
  components: {
    'show-more': showMore
  },
  data () {
    return {
      api_key: '3e6f136790a44d598d5922439e6d8711',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      className: '',
      view: false,
      buttonText: 'Show More Content',
      enterValidLocation: false,
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        this.enterValidLocation = false;
        this.buttonText = 'Show More Content';

        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          })
          .then(this.setResults)
          .catch(err => {
            this.enterValidLocation = true;
            console.log(err)
          });
      }
    },
    setResults (results) {
      this.weather = results;
      if(this.weather && this.weather.main) {
        this.className = (this.weather.main.temp > 20) ? 'warm' : '';
        console.log('tempt is '+ results)
        this.enterValidLocation = false;
      } else {
        this.enterValidLocation = true;
        this.className = 'space';
      }
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
    doActions() {
        if(!this.enterValidLocation) {
          this.view = (this.view == true) ? false : true
          
          if(this.view) {
            this.buttonText = 'Show Less Content'
          } else {
            this.buttonText = 'Show More Content'
          }

          setTimeout(() => {
            window.scrollTo(0, window.innerHeight + 50); 
          }, 300);
        }
        // v-on:click="() => this.view = (this.view == true) ? false : true" 
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  scroll-behavior: smooth;
}
body {
  font-family: 'montserrat', sans-serif;

}
#app {
  background-image: url('./assets/weather-snowy.jpg');
  background-size: cover;
  background-position: center;
  transition: 1s;
  background-attachment: fixed;
}
#app.warm {
  background-image: url('./assets/weather-sunny.jpg');
}
#app.space {
  background-image: url('./assets/space.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  display: flex;
  flex-direction: column;
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location, .enterLocation {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.showMore {
  z-index: 10;
  margin-bottom: 5rem;
  box-sizing: border-box;
}
div.hasShowMoreButton {
  width: 100vw;
}
.showMoreButton {
  z-index: 11;
  width: 95vw;
  
  padding: 15px 0;
  margin-top: 1rem;
  
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  border-radius: 0px 16px 0px 16px;
  
  background:rgba(0, 0, 0, 0.75);
  color: white;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);

  position: fixed;
  left: 2.5vw !important;
  bottom: 1rem !important;
  display: block;

  cursor: pointer;
  transition: 0.4s;
}
show-more {
  background:rgba(255, 255, 255, 0.25);
  color: white;
}
.hide {
  display: none;
}
.show {
  display: block;
  scroll-margin-top: 5000px;
}
</style>
