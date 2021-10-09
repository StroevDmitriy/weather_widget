<template>
  <!-- <MDBBtn color="success">Success</MDBBtn> -->
  <div class="custom-container">
    <Settings>
      <button>
        <MDBIcon icon="cog" iconStyle="fas" />
      </button>
    </Settings>

    <CityInfo />
      
    <CityInfo />
  </div>
</template>

<script>
import CityInfo from './components/CityInfo.vue'
import Settings from './components/Settings.vue'

export default {
  name: 'App',
  components: {
    CityInfo,
    Settings
  }
}
</script>

<style>
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
</style>
