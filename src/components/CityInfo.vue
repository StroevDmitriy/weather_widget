<template>
    <div class="city-card">
        <h2>City Info Component</h2>
        <div class="location">
            <span class="city">City name</span>, 
            <span class="country">Country code</span>
        </div>
        <div class="main-info">
            <div class="weather-ico">Weather icon</div>
            <span class="tmprt">7°</span>
        </div>
        <p class="brif">
            <span>Feels like</span>. <span>Clouds</span>. <span>Wind</span>
        </p>
        <div class="details">
            <div class="wind-direction">Wind direction</div>
            <div class="pressure">Pressure</div>
            <div class="humidity">Humidity</div>
            <div class="visibility">Visibility</div>
        </div>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>
.city-card{
    padding: 15px;
}
.location{
    display: flex;
}
.main-info{
    display: flex;
}
.brif{
    text-align: left;
}
.details{
    display: flex;
    justify-content: space-between;
}
</style>