<template>
  <!-- <MDBBtn color="success">Success</MDBBtn> -->
  
  <Settings>
    <button>
      <MDBIcon icon="cog" iconStyle="fas" />
    </button>
  </Settings>

  <CityInfo>
    
  </CityInfo>
</template>

<script>
// import { MDBBtn } from "mdb-vue-ui-kit";
import { MDBIcon } from "mdb-vue-ui-kit";
import CityInfo from './components/CityInfo.vue'
import Settings from './components/Settings.vue'

export default {
  name: 'App',
  components: {
    // MDBBtn,
    MDBIcon,
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
}
</style>
