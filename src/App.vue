<template>
  <div class="app-container">
    <button class="open-settings" type="button" @click="openSettings">
        <MDBIcon icon="cog" iconStyle="fas" v-if="!settingsVisible"/>
        <MDBIcon fas icon="times" v-else/>
    </button>

    <Settings :cityList="cityList" :apiKey="apiKey" v-show="settingsVisible" @removeCity="removeCity" @addCity="addCity" />

    <template v-for="(city,id) in cityList" :key="id">
      <CityInfo :city="city"/>  
    </template>
  </div>
</template>

<script>
import CityInfo from './components/CityInfo.vue'
import Settings from './components/Settings.vue'
import { MDBIcon } from "mdb-vue-ui-kit";

export default {
  name: 'App',
  data() {
    return {
      settingsVisible: false,
      apiKey: '4d8d0d73051f578e2b7a85919fe8c011',
      cityList: null,
    }
  },
  computed: {
    
  },
  components: {
    CityInfo,
    Settings,
    MDBIcon
  },
  methods: {
    openSettings() {
      this.settingsVisible = !this.settingsVisible;
    },
    createUrl(cityName, apiKey) {
      return `http://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${apiKey}&units=metric`
    },
    addCity(cityName) {
      if (this.cityList == null) {
        this.cityList = new Array;
      }
      fetch(this.createUrl(cityName, this.apiKey))
        .then(response => response.json())
        .then(result => {
          this.cityList.push(result);
        });
    },
    removeCity(idx) {
      this.cityList.splice(idx, 1);
    },
  },
  created() {
    this.cityList = new Array;

    //Add current geoposition
    let ctxt = this;
    navigator.geolocation.getCurrentPosition(success, error, {enableHighAccuracy: true})

    function success({ coords }) {
      const { latitude, longitude } = coords
      fetch(`http://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${ctxt.apiKey}&units=metric`)
        .then(response => response.json())
        .then(result => {
          ctxt.cityList.push(result);
        });
    }

    function error({ message }) {
      console.log(message) // if access denied get PositionError: User denied Geolocation
    }

    //Add another cities
    this.addCity('Moscow');
    this.addCity('Saint Petersburg');
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  max-width: 320px;
  margin-left: auto;
  margin-right: auto;
}

.app-container{
  position: relative;
  display: flex;
  flex-flow: column;
}

.open-settings{
  margin-left: auto;
  background: none;
  border: 1px solid #000;
  border-radius: 50%;
  height: 40px;
  width: 40px;
}
</style>
