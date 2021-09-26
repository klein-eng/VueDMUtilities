<template>
    <expandable :hidden="hidden">
        <div class="expanded-content" :class="{hidden: this.hidden}">
            <input v-if="editMode" :value="actor.AC" @input="updateActorValue('AC', $event.target.value)" placeholder="AC"/>
            <div v-else-if="actor.AC !== undefined">AC: {{actor.AC}}</div>

            <input v-if="editMode" :value="actor.Status" @input="updateActorValue('Status', $event.target.value)" placeholder="Status"/>
            <div v-else-if="actor.Status !== undefined">Status: {{actor.Status}}</div>
        </div>
        <div>
            <input placeholder="Notes" class="notes-field">
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
    emits: ["updateActor"]
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
    }
    .notes-field {
        width: 100%;
        margin-bottom: 5px;
    }
</style>