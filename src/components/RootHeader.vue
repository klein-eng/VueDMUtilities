<template>
    <div id="utility-header">
        <button id="utility-button" class="button-size">
            <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="16" height="16" viewBox="0 0 16 16">
                <path fill="#000000" d="M1 3h14v3h-14zM1 7h14v3h-14zM1 11h14v3h-14z"></path>
            </svg>
        </button>
        <h2 id="utility-title">Initiative Tracker</h2>
        <div class="button-size"/>
    </div>
</template>

<script>
export default {
    name: 'RootHeader'
}
</script>

<style lang="scss" scoped>
    @import '../sass/variables.scss';
    #utility-header {
        display: flex;
    }
    #utility-title {
        margin-top: 10px;
        flex-grow: 1;
    }
    #utility-button {
        background-color: $primary;
        border: solid 2px $white;
        border-radius: 5px;
        margin: 5px;

        svg {
            height: 25px;
            width: 25px;
        }
    }
    .button-size {
        height: 35px;
        width: 40px;
        margin: 5px;
    }
</style>