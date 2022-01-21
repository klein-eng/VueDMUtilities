<template>
    <modal :active="this.actor !== null">
        <div>
            <div>Modifier: <input type="number" v-model="initMod" placeholder="Modifier"/></div>
            <div>Duplicates: <input type="number" v-model="numDuplicates" placeholder="Duplicates"/></div>
            <button @click="duplicate">Duplicate</button>
            <button @click="cancel">Cancel</button>
        </div>
    </modal>
</template>

<script>
import Modal from './Generic/Modal.vue'

export default {
    props: {
        actor: Object
    },
    components: {
		Modal
	},
    data () {
        return {
            numDuplicates: 1,
            initMod: null
        }
    },
    methods: {
        duplicate () {
            this.$emit('duplicate', this.numDuplicates, this.initMod);
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
    div {
        font-size: 16px;
		font-weight: bold;
        margin-top: 5px;
    }
    button {
        background-color: $primary;
        border: solid 2px $white;
        border-radius: 5px;
        font-size: 18px;
        font-weight: bold;
        margin: 4px;
    }
</style>