<template>
    <div class="settings-panel">
        <div class="settings-card">
            <h3>Settings</h3>
            <ul class="current-cities">
                <li draggable="true" v-for="(city, id) in cityList" :key="id">
                    <button class="dnd"><MDBIcon fas icon="bars" /></button>
                    <h4>{{ city.name }}</h4>
                    <button
                        type="button"
                        class="remove-btn"
                        @click="$emit('removeCity', id), clearSearch()"
                    ><MDBIcon icon="trash" iconStyle="fas" />
                    </button>
                </li>
            </ul>
            <div class="add-location">
                <h3>Add location:</h3>
                <div class="search-block">
                    <div class="search-block__input-n-result">
                        <input
                            class="city-search"
                            type="text"
                            placeholder="New York"
                            v-model="citySearch">
                        <div class="suggest visible" v-if="foundCity">
                            <span>{{ foundedCityName }}</span>
                        </div>
                    </div>

                    <button
                        class="search-btn"
                        @click="$emit('addCity', foundCity.name), clearSearch()"
                    ><MDBIcon fas icon="level-down-alt"/></button>
                </div>
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
                foundCity: null,
                // suggestVisibility: false,
            }
        },
        computed: {
            foundedCityName() {
                console.log(this.foundCity);
                if (this.foundCity.name && this.foundCity.sys.country) {
                    return `${this.foundCity.name}, ${this.foundCity.sys.country}`;
                } else {
                    return 'Not found';
                }
            },
        },
        emits: ['removeCity', 'addCity'],
        components: {
            MDBIcon,
        },
        methods: {
            clearSearch() {
                this.citySearch = null;
                this.foundCity = null;
                // this.suggestVisibility = false;
            }
        },
        watch: {
            citySearch(newValue, ) {
                // console.log(newValue);
                clearTimeout(this.timerId);

                if (newValue == '') {
                    this.foundCity = null;
                } else {
                    let ctxt = this;
                    this.timerId = setTimeout(function() {
                        fetch(`http://api.openweathermap.org/data/2.5/weather?q=${newValue}&appid=${ctxt.apiKey}&units=metric`)
                            .then(response => response.json())
                            .then(function (result) {
                                ctxt.foundCity = result;
                                ctxt.suggestVisibility = true;
                            })
                            .catch(error => {
                                console.log('Error!!!');
                                console.error('Add city error: ', error);
                                ctxt.foundCity = null;
                            });
                    }, 1500);
                }
                
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
.search-block{
    display: flex;
    align-items: flex-start;

    &__input-n-result{
        position: relative;
        margin-right: 10px;
        margin-bottom: 36px;
    }
}
.city-search{
    width: 100%;
}
.suggest{
    position: absolute;
    display: none;
    width: 100%;
    height: 36px;
    top: 100%;
    left: 0;
    align-items: center;
    padding: 6px;
    border: 1px solid red;
    border-top: none;
}
.city-search:focus + .suggest{
    display: flex;
}
.search-btn{
    background: none;
    border: none;

    .fas{
        transform: rotate(90deg);
    }
}
</style>