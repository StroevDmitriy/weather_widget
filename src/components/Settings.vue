<template>
    <div class="settings-row">
        <div class="settings-card">
            <h3>Settings</h3>

            <ul class="current-cities">
                <li>
                    <h4>London, UK</h4>
                    <button type="button"><MDBIcon icon="trash" iconStyle="fas" /></button>
                </li>
                <li>
                    <h4>Moscow, RU</h4>
                    <button type="button"><MDBIcon icon="trash" iconStyle="fas" /></button>
                </li>
            </ul>
            <div class="add-location">
                <h3>Add location</h3>
                <input type="text" placeholder="New York">
            </div>
        </div>
        
        <button class="open-settings" type="button">
            <MDBIcon icon="cog" iconStyle="fas" />
        </button>
    </div>
</template>

<script>
    import { MDBIcon } from "mdb-vue-ui-kit";
    export default {
        components: {
            MDBIcon,
        }
    }
</script>

<style lang="scss" scoped>
.settings-row{
    display: flex;
    justify-content: flex-end;
}
.open-settings{
    width: 32px;
    height: 32px;
}
.settings-card{
    padding: 15px;
}
.current-cities{
    li{
        display: flex;
    }
}
</style>