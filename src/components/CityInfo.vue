<template>
    <div class="city-card">
        <h3 class="location">
            <span class="city">
                {{ city.name }}
            </span>, 
            <span class="country">{{ city.sys.country }}</span>
        </h3>
        <div class="main-info">
            <div class="weather-ico">
                <img :src="`http://openweathermap.org/img/wn/${city.weather[0].icon}@2x.png`" alt="">
            </div>
            <span class="tmprt">{{ Math.round(city.main.temp) }}°</span>
        </div>
        <p class="brif">
            <span>Feels like {{ Math.round(city.main.feels_like) }}°</span>. <span>{{ city.weather[0].main }}</span>.
        </p>
        <div class="details">
            <div class="wind-direction"><span><MDBIcon fas icon="location-arrow" /></span> {{ Math.round(this.city.wind.speed * 10) / 10 }} m/s {{ windDirection }}</div>
            <div class="pressure"><span><MDBIcon far icon="compass" /></span> {{ city.main.pressure }} hPa</div>
            <div class="humidity">Humidity: {{ city.main.humidity }}%</div>
            <div class="visibility">Visibility: {{(Math.round(city.visibility / 100) / 10).toFixed(1)}} km</div>
        </div>
    </div>
</template>

<script>
    import { MDBIcon } from "mdb-vue-ui-kit";
    export default {
        props: {
            city: {
                type: Object,
            }
        },
        computed: {
            windDirection() {
                if (this.city.wind.deg <= 11.25) { return 'N'; }
                if (11.25 < this.city.wind.deg && this.city.wind.deg < 33.75) { return 'NNE'; }
                if (33.75 <= this.city.wind.deg && this.city.wind.deg <= 56.25) { return 'NE'; }
                if (56.25 < this.city.wind.deg && this.city.wind.deg < 78.75) { return 'ENE'; }
                if (78.75 <= this.city.wind.deg && this.city.wind.deg <= 101.25) { return 'E'; }
                if (101.25 < this.city.wind.deg && this.city.wind.deg < 123.75) { return 'ESE'; }
                if (123.75 <= this.city.wind.deg && this.city.wind.deg <= 146.25) { return 'SE'; }
                if (146.25 < this.city.wind.deg && this.city.wind.deg < 168.75) { return 'SSE'; }
                if (168.75 <= this.city.wind.deg && this.city.wind.deg <= 191.25) { return 'S'; }
                if (191.25 < this.city.wind.deg && this.city.wind.deg < 213.75) { return 'SSW'; }
                if (213.75 <= this.city.wind.deg && this.city.wind.deg <= 236.25) { return 'SW'; }
                if (236.25 < this.city.wind.deg && this.city.wind.deg < 258.75) { return 'WSW'; }
                if (258.75 <= this.city.wind.deg && this.city.wind.deg <= 281.25) { return 'W'; }
                if (281.25 < this.city.wind.deg && this.city.wind.deg < 303.75) { return 'WNW'; }
                if (303.75 <= this.city.wind.deg && this.city.wind.deg <= 326.75) { return 'NW'; }
                if (326.75 <= this.city.wind.deg && this.city.wind.deg <= 348.75) { return 'NNW'; }
                if (348.75 <= this.city.wind.deg) { return 'N'; }
                else { return '-'; }
            }
        },
        components: {
            MDBIcon
        }
    }
</script>

<style lang="scss" scoped>
h3{
    font-size: 20px;
}
.city-card{
    padding: 15px;
    margin-bottom: 50px;
}
.location{
    display: flex;
}
.main-info{
    display: flex;
}
.tmprt{
    font-size: 52px;
}
.brif{
    text-align: left;
}
.details{
    display: flex;
    flex-flow: wrap;
    justify-content: space-between;
}
</style>