<template>
    <div v-if="this.actor !== null" class="shield">
        <div class="modal">
            <input type="number" v-model="numDuplicates" />
            <button @click="duplicate">Duplicate</button>
            <button @click="cancel">Cancel</button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        actor: Object
    },
    data () {
        return {
            numDuplicates: 1
        }
    },
    methods: {
        duplicate () {
            this.$emit('duplicate', this.numDuplicates);
            this.$emit('update:actor', null);
        },
        cancel () {
            this.$emit('update:actor', null);
        }
    },
    emits: ['update:actor', 'duplicate']
}
</script>

<style lang="scss" scoped>
    @import '../sass/variables.scss';
    .modal {
        position: absolute;
        background-color: $background;
        border: 2px solid $white;
        border-radius: 4px;
        top: 50%;
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