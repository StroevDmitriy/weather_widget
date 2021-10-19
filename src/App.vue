<template>
  <div class="app-container" v-cloak>
    <h1>Weather widget</h1>
    <button class="open-settings" type="button" @click="switchSettingsPanel">
        <MDBIcon icon="cog" iconStyle="fas" v-if="!settingsVisible"/>
        <MDBIcon fas icon="times" v-else/>
    </button>

    <Settings :cityList="cityList" :apiKey="apiKey" v-show="settingsVisible" @removeCity="removeCity" @addCity="addCity" />

    <div class="city-list" v-show="!settingsVisible">
      <template v-for="(city,id) in cityList" :key="id">
        <CityInfo :city="city"/>  
      </template>
    </div>
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
      cityList:
        (JSON.parse(localStorage.getItem('cityList')) && JSON.parse(localStorage.getItem('cityList')).length != 0) ?
        JSON.parse(localStorage.getItem('cityList')) : new Array,
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
    switchSettingsPanel() {
      this.settingsVisible = !this.settingsVisible;
    },
    createUrl(cityName, apiKey) {
      return `http://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${apiKey}&units=metric`
    },
    addCity(cityName) {
      fetch(this.createUrl(cityName, this.apiKey))
        .then(response => response.json())
        .then(result => {
          this.cityList.push(result);
          localStorage.setItem('cityList', JSON.stringify(this.cityList))
        });

    },
    removeCity(idx) {
      this.cityList.splice(idx, 1);
      localStorage.setItem('cityList', JSON.stringify(this.cityList))

      this.cityList = JSON.parse(localStorage.getItem('cityList'));
    },
  },
  created() {
    let ctxt = this;

    function success({ coords }) {
      const { latitude, longitude } = coords
      fetch(`http://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${ctxt.apiKey}&units=metric`)
        .then(response => response.json())
        .then(result => {
          ctxt.addCity(result.name);
        });
    }

    function error({ message }) {
      console.log(message) // if access denied get PositionError: User denied Geolocation
    }

    if (this.cityList.length == 0) {
      //Add current geoposition
      navigator.geolocation.getCurrentPosition(success, error, {enableHighAccuracy: true});

      //Add another cities
      this.addCity('Moscow');
      this.addCity('Saint Petersburg');
    }
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
  padding-top: 60px;
  padding: 60px 15px 50px;

  max-width: 320px;
  margin-left: auto;
  margin-right: auto;
}

.app-container{
  position: relative;
  display: flex;
  flex-flow: column;
}

h1{
  margin-bottom: 40px;
}

.open-settings{
  margin-left: auto;
  height: 40px;
  width: 40px;
  margin-bottom: 20px;
  border: none;
  border-radius: 50%;
  background: none;
  transition: all .3s;

  &:hover{
    background-color: rgba(0,0,0,.05);
  }
}

[v-cloak]{
  display: none;
}
</style>
