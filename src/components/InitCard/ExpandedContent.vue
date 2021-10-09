<template>
    <expandable :hidden="hidden">
        <div class="expanded-content" :class="{hidden: this.hidden}">
            <input v-if="editMode" :value="actor.AC" @input="updateActorValue('AC', $event.target.value)" placeholder="AC" class="AC-input"/>
            <div v-else-if="actor.AC !== undefined">AC: {{actor.AC}}</div>

            <input v-if="editMode" :value="actor.Status" @input="updateActorValue('Status', $event.target.value)" placeholder="Status"/>
            <div v-else-if="actor.Status !== undefined">Status: {{actor.Status}}</div>
        </div>
        <div>
            <input placeholder="Notes" class="notes-field">
        </div>
        <div>
            <button @click="this.$emit('duplicate')">Duplicate</button>
            <button @click="this.$emit('delete')" class="red-button">Delete</button>
        </div>
    </expandable>
</template>

<script>
import Expandable from '../Utilities/Expandable.vue'

export default {
    name: "ExpandedContent",
    components: {
        Expandable
    },
    props: {
        actor: Object,
        editMode: Boolean,
        hidden: Boolean,
    },
    methods: {
        updateActorValue: function(key, value) {
			this.$emit("updateActor", key, value);
		},
    },
    emits: ["updateActor","delete","duplicate"]
}
</script>

<style lang="scss" scoped>
	@import '../../sass/variables.scss';
	input {
		background-color: $white;
		color: $black;
		border: none;
		border-radius: 6px;
		font-size: 16px;
		font-weight: bold;
		text-align: center;
		padding-top: $card-padding;
		padding-bottom: $card-padding;
		margin-right: 5px;
	}
    button {
        font-size: 18px;
		font-weight: bold;
        background-color: $background;
        color: $white;
        border: none;
        border-radius: 5px;
        margin: 0 5px;
        &.red-button {
            background-color: darkred;
        }
    }
    .expanded-content {
        margin-top: 5px;
        margin-bottom: 5px;
        display: flex;
        padding-top: 5px;
        border-top: dashed 2px $white;
        .hidden {
            border-top: none;
        }
        div, input {
            flex-grow: 1;
            width: 100%;
        }
        .AC-input {
            flex-grow: 0;
            flex-basis: 60px;
        }
    }
    .notes-field {
        width: 95%;
        margin-bottom: 5px;
    }
</style>