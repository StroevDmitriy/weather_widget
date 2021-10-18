<template>
    <div class="settings-panel">
        <div class="settings-card">
            <h3>Settings</h3>
            <ul class="current-cities">
                <li draggable="true" v-for="(city, id) in cityList" :key="id">
                    <button class="dnd"><MDBIcon fas icon="bars" /></button>
                    <h4>{{ city.name }}</h4>
                    <button type="button" class="remove-btn" @click="$emit('removeCity', id)">
                        <MDBIcon icon="trash" iconStyle="fas" />
                    </button>
                </li>
            </ul>
            <div class="add-location">
                <h3>Add location: {{ citySearch }}</h3>
                <input type="text" placeholder="New York" v-model="citySearch">
            </div>
        </div>
    </div>
</template>

<script>
    import { MDBIcon } from "mdb-vue-ui-kit";
    export default {
        props: {
            cityList: {
                type: Object,
            },
            apiKey: {
                type: String,
            }
        },
        data() {
            return{
                citySearch: null,
                timerId: null,
            }
        },
        emits: ['removeCity'],
        components: {
            MDBIcon,
        },
        methods: {

        },
        watch: {
            citySearch(newValue, oldValue, ) {
                clearTimeout(this.timerId);

                let apiKey = this.apiKey
                this.timerId = setTimeout(function() {
                    console.log(`http://api.openweathermap.org/data/2.5/weather?q=${newValue}&appid=${apiKey}&units=metric`);
                    console.log(newValue, oldValue);
                }, 1500);

                
                // `http://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${apiKey}&units=metric`

                // fetch(`http://api.openweathermap.org/data/2.5/weather?q=${newValue}&appid=${this.apiKey}&units=metric`)
                //     .then(response => response.json())
                //     .then(result => {
                    //     this.cityList.push(result);

                //     console.log(result);
                //     })
            },
        },
    }
</script>

<style lang="scss" scoped>
.settings-panel{
    display: flex;
    justify-content: center;
    position: absolute;
    height: 100%;
    width: 100%;
    background-color: #fff;
    top: 50px;
}
.open-settings{
    width: 32px;
    height: 32px;
}
.settings-card{
    padding: 15px;
}
.current-cities{
    padding-left: 0px;
    margin-bottom: 50px;

    h4{
        margin: 0px 10px 0px 0px;
        font-size: 20px;
    }

    li{
        display: flex;
        width: 100%;
        margin: 8px 0px;
        padding: 10px 8px;
        // justify-content: space-between;
        background-color: #eee;
    }
}
.dnd{
    border: none;
    padding: 0px;
    margin-right: 10px;
}
.remove-btn{
    border: none;
    background: none;
    margin-left: auto;
}
</style>