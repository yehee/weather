<template>
  <div class="weather-forcast">
    <!-- <img :src="logo"> -->
    <img :src="url">
    <p>Latitude: {{ lat }}</p>
    <p>Longitude: {{ lon }}</p>
    <button id="btn-v" @click="sendRealtimeData()">How's the weather like in Vancouver?</button>
    <button id="btn-u" @click="sendForecastData()">How's the weather like in your location?</button>
    <label>Latitude</label>
    <input type="text" v-model="latitude">
    <label>Longitude</label>
    <input type="text" v-model="longitude">
    <button id="btn-r" @click="returnToStart()">&larr; Go back</button>
    <p>Today's temperature is {{ temp }} {{ units }}</p>
  </div>
</template>

<script>
import axios from 'axios'

// var t1 = new Date()
// var t2 = new Date()
// t2.setHours(t2.getHours() + 1)
// console.log(t1.toISOString())
// console.log(t2.toISOString())

export default {
  name: 'HelloWorld',
  data () {
    return {
      url: require('../assets/flower.png'),
      lat: '',
      lon: '',
      latitude: '',
      longitude: '',
      temp: '',
      units: ''
    }
  },
  //   mounted() {
  //     axios({
  //       method: 'GET',
  //       url: 'https://httpbin.org/ip' }).then(result => {
  //       this.ip = result.data.origin;
  //     }, error => {
  //       console.error(error);
  //     });
  //   },
  methods: {
    cleanUp () {
      var inputs = document.getElementsByTagName('p')
      Array.prototype.forEach.call(inputs, (el, index, array) => {
        el.style.display = 'block'
      })
      document.getElementById('btn-v').style.display = 'none'
      document.getElementById('btn-u').style.display = 'none'
      document.getElementById('btn-r').style.display = 'block'
    },
    sendRealtimeData () {
      axios({
        method: 'POST',
        'url': 'https://api2.climacell.co/v2/realtime',
        'data': {
          'geocode': {
            'lon': -123.120738,
            'lat': 49.282729
          },
          'location_id': '',
          'fields': [ {
            'name': 'temp',
            'units': 'C'
          } ]},
        'headers': { 'content-type': 'application/json', 'apikey': 'NONryKyas8rLlYvCMH8OhkbhD1D9TV3Z' }
      }).then(result => {
        this.lat = result.data.lat
        this.lon = result.data.lon
        this.temp = result.data.temp.value
        if (this.temp > 20) {
          this.url = require('../assets/sun.png')
        } else {
          this.url = require('../assets/rain.png')
        }
        this.units = result.data.temp.units
        this.cleanUp()
      }, error => {
        console.error(error)
      })
    },
    sendForecastData () {
      axios({
        method: 'POST',
        'url': 'https://api2.climacell.co/v2/realtime',
        'data': {
          'geocode': {
            'lon': Number(this.longitude),
            'lat': Number(this.latitude)
          },
          'location_id': '',
          'fields': [ {
            'name': 'temp',
            'units': 'C'
          } ]},
        'headers': { 'content-type': 'application/json', 'apikey': 'NONryKyas8rLlYvCMH8OhkbhD1D9TV3Z' }
      }).then(result => {
        this.lat = result.data.lat
        this.lon = result.data.lon
        this.temp = result.data.temp.value
        if (this.temp > 20) {
          this.url = require('../assets/sun.png')
        } else {
          this.url = require('../assets/rain.png')
        }
        this.units = result.data.temp.units
        this.cleanUp()
      }, error => {
        console.error(error)
      })
    },
    returnToStart () {
      location.reload()
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
div p {
  display: none;
}
label {
  position: absolute;
  transition: .4s cubic-bezier(.25,.8,.25,1)
}
input[type="text"] {
  border: none;
  border-bottom: 1px solid seagreen;
  margin-top: 5px;
}
label:hover {
  transform: translateY(-18px) scale(.75);
}
button {
  padding: 7px 14px;
  border-radius: 15px;
  background: #d3f2ff;
  border: none;
  font-size: medium;
  color: #2c3e50;
  cursor: pointer;
  display: block;
  margin: 20px auto;
  transition: background 0.5s;
}
#btn-v {
  background: #c1e8c0;
}
#btn-v:hover {
  background: #a7d671;
}
#btn-u {
  background: pink;
}
#btn-u:hover {
  background: #ffa4bd;
}
#btn-r {
  display: none;
}
* {
  outline: none;
}
</style>
