<template>
    <expandable :hidden="hidden">
        <div class="expanded-content" :class="{hidden: this.hidden}">
            <input v-if="editMode" :value="actor.AC" @input="updateActorValue('AC', $event.target.value)" placeholder="AC" class="AC-input" :tabindex="tabIndex"/>
            <div v-else-if="actor.AC !== undefined">AC: {{actor.AC}}</div>

            <input v-if="editMode" :value="actor.Status" @input="updateActorValue('Status', $event.target.value)" placeholder="Status" :tabindex="tabIndex"/>
            <div v-else-if="actor.Status !== undefined">Status: {{actor.Status}}</div>

            <div v-if="editMode" class="checkbox-container">
                <label>
                    <input type="checkbox" :value="actor.IsPC" @input="updateActorValue('IsPC', !isPC)" :tabindex="tabIndex" :checked="actor.IsPC"/>
                    PC
                </label>
            </div>
        </div>
        <div>
            <input placeholder="Notes" class="notes-field" :tabindex="tabIndex">
        </div>
        <div v-if="!editMode" class="buttonrow">
            <button @click="this.$emit('duplicate')" class="duplicate-button" :tabindex="tabIndex">Duplicate</button>
            <button @click="this.$emit('delete')" class="delete-button" :tabindex="tabIndex">Delete</button>
        </div>
    </expandable>
</template>

<script>
import Expandable from '../Generic/Expandable.vue'

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
    computed: {
		tabIndex: function () {
			if (this.hidden) {
				return "-1";
			}
			return "0";
		},
        isPC: function() {
            return this.actor.IsPC === true;
        }
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
        border-radius: 4px;
        margin: 0px 5px;
        margin-bottom: 3px;
        padding: 5px 10px;
        &.delete-button {
            background-color: darkred;
        }
    }
    .buttonrow {
        float: right;
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
        .checkbox-container {
            flex-grow: 0;
            max-width: auto;
            flex-basis: 100px;
            input {
                width: auto;
            }
        }
    }
    .notes-field {
        width: 95%;
        margin-bottom: 5px;
    }
</style>