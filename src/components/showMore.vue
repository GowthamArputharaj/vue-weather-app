<template>
  <div>
    <ul>
      <div 
        class="showContent"
        v-for="(k, content) in arr" v-bind:key="content"
        v-on:click="showProperties(Object.values(k))">
        <h2><span>{{k[0]}}</span> <span>{{k[1]}}</span></h2>
      </div>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    weather: Object
  },
  data() {
    return {
      arr: [],
    }
  },
  created() {
    this.arr.push(['Description' , this.weather.weather[0].description])
    this.arr.push(['Latitude' , this.weather.coord.lat + ' °'])
    this.arr.push(['Longitude' , this.weather.coord.lon + ' °'])
    this.arr.push(['Pressure' , this.weather.main.pressure + ' pascal'])
    this.arr.push(['Humidity' , this.weather.main.humidity + ' %'])
    this.arr.push(['Minimum Temparature' , this.weather.main.temp_min + ' °C'])
    this.arr.push(['Maximum Temparature' , this.weather.main.temp_max + ' °C'])
    this.arr.push(['Wind Speed' , this.weather.wind.speed + ' mph'])
    this.arr.push(['Wind direction' , this.weather.wind.deg])
  },
  methods: {
    showProperties(v) {
      this.val = v
      return this.val;
    }
  }
};
</script>

<style scoped>
.showContent {
  text-shadow: 2px 3px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.15);
  box-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  padding: 0.1rem 1rem;
  border-radius: 8px;
  margin-top: 0.7rem;
}
h2 {
  font-weight: 500;
  display: flex;
  flex-direction: row;
  color: white;
  list-style-type: none;
  margin: 1rem 0;
}
span {
  justify-content: center;
  align-items: center;
  flex: 0 50%;
}
</style>
