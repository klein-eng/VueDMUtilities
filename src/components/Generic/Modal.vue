<template>
    <div v-if="active" class="shield">
        <div class="modal">
            <slot></slot>
        </div>
    </div>
</template>

<script>
	export default {
        name: "Modal",
        props: {
            active: Boolean
        }
    }
</script>

<style lang="scss" scoped>
    @import '../../sass/variables.scss';
    .modal {
        position: absolute;
        background-color: $background;
        padding: 5px;
        border: 2px solid $white;
        border-radius: 4px;
        top: 90px;
        width: 90%;
        left: 5%;
    }
    .shield {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba($color: $black, $alpha: 0.75);
    }
</style>