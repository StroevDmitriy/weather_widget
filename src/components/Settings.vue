<template>
    <div class="settings-panel">
        <!-- <div class="settings-card"> -->
            <h3>Settings</h3>
            <ul class="current-cities">
                <li draggable="true" v-for="(city, id) in cityList" :key="id">
                    <button class="dnd"><MDBIcon fas icon="bars" /></button>
                    <h4>{{ city.name }}</h4>
                    <button
                        type="button"
                        class="remove-btn"
                        @click="$emit('removeCity', id)"
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
                        <div class="suggest visible" v-if="suggestVisible">
                            <span>{{ foundedCityName }}</span>
                        </div>
                    </div>

                    <button
                        class="add-city-btn"
                        :class="{'disabled': btnAddDisabled, 'yee': true}"
                        @click="$emit('addCity', foundCity.name), clearSearch()"
                    ><MDBIcon fas icon="level-down-alt"/></button>
                </div>
            </div>
        <!-- </div> -->
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
                suggestVisible: false,
                btnAddDisabled: false,
            }
        },
        computed: {
            foundedCityName() {
                if (this.foundCity !== null && this.foundCity.name && this.foundCity.sys.country) {
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
                this.suggestVisible = false;
            }
        },
        watch: {
            citySearch(newValue) {
                clearTimeout(this.timerId);

                if (newValue == '') {
                    this.suggestVisible = null;
                } else {
                    let ctxt = this;
                    this.btnAddDisabled = true;
                    this.suggestVisible = false;
                    this.timerId = setTimeout(async function() {
                        let response = await fetch(`http://api.openweathermap.org/data/2.5/weather?q=${newValue}&appid=${ctxt.apiKey}&units=metric`)

                        if (response.ok) {
                            let result = await response.json();

                            ctxt.foundCity = result;
                            ctxt.btnAddDisabled = false;
                        }
                        else {
                            ctxt.foundCity = null;
                        }
                        ctxt.suggestVisible = true;
                    }, 500);
                }
            },
        },
    }
</script>

<style lang="scss" scoped>
.settings-panel{
    display: flex;
    flex-flow: column;
    padding: 15px;
    height: 100%;
    width: 100%;
    top: 50px;
    justify-content: center;
    padding-bottom: 60px;
    background-color: #fff;

    border-radius: 20px;
    border: 1px solid #000;
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
    background-color: #dedede;
    border-top: none;
}
.city-search:focus + .suggest{
    display: flex;
}
.add-city-btn{
    background: none;
    border: none;

    &.disabled{
        opacity: .5;
        pointer-events: none;
    }

    .fas{
        transform: rotate(90deg);
    }
}
</style>