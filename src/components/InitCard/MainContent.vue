<template>
	<div class="main-content">
		<div class="init-counter">
			<input v-if="editMode" :value="actor.init" type='number' @input="updateActorValue('init', this.numberOrNull($event.target.value))" @keyup.enter='onEnterKeyUp()' placeholder="Init" class="init-counter-edit"/>
			<div v-else @click="$emit('toggleSelected')" class="init-counter-label">{{ actor.init }}</div>
		</div>
		<div class="name">
			<input v-if="editMode" :value="actor.name" @input="updateActorValue('name', $event.target.value)" placeholder="Name" class="name-edit"/>
			<div v-else class="name-label">
				<svg v-if="isCurrentTurn" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="16" height="16" viewBox="0 0 16 16">
					<path fill="#000000" d="M0 0h2v16h-2v-16z"></path>
					<path fill="#000000" d="M13 10.047c1.291 0 2.415-0.312 3-0.773v-8c-0.585 0.461-1.709 0.773-3 0.773s-2.415-0.312-3-0.773v8c0.585 0.461 1.709 0.773 3 0.773z"></path>
					<path fill="#000000" d="M9.5 0.508c-0.733-0.312-1.805-0.508-3-0.508-1.506 0-2.818 0.312-3.5 0.773v8c0.682-0.461 1.994-0.773 3.5-0.773 1.195 0 2.267 0.197 3 0.508v-8z"></path>
				</svg>
				{{ actor.name }}
			</div>
		</div>
		<div v-if="actor.maxHP && !editMode" class="hitpoints">{{actor.curHP}} / {{actor.maxHP}}</div>
		<input v-else-if="editMode" :value="actor.maxHP" @input="updateActorValue('maxHP', $event.target.value)" class="hp-edit" placeholder="HP" />
		
		<button v-if="isDefeated" @click="this.$emit('delete')" class="icon-button destructive">Delete</button>

		<button v-if="editMode" @click="this.$emit('toggleEdit')" class="icon-button done">
			<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 16 16">
				<path d="M13.5 2l-7.5 7.5-3.5-3.5-2.5 2.5 6 6 10-10z"></path>
			</svg>
		</button>
		<button v-else @click="this.$emit('toggleEdit')" class="icon-button">
			<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 16 16">
				<path d="M14.59 9.535c-0.839-1.454-0.335-3.317 1.127-4.164l-1.572-2.723c-0.449 0.263-0.972 0.414-1.529 0.414-1.68 0-3.042-1.371-3.042-3.062h-3.145c0.004 0.522-0.126 1.051-0.406 1.535-0.839 1.454-2.706 1.948-4.17 1.106l-1.572 2.723c0.453 0.257 0.845 0.634 1.123 1.117 0.838 1.452 0.336 3.311-1.12 4.16l1.572 2.723c0.448-0.261 0.967-0.41 1.522-0.41 1.675 0 3.033 1.362 3.042 3.046h3.145c-0.001-0.517 0.129-1.040 0.406-1.519 0.838-1.452 2.7-1.947 4.163-1.11l1.572-2.723c-0.45-0.257-0.839-0.633-1.116-1.113zM8 11.24c-1.789 0-3.24-1.45-3.24-3.24s1.45-3.24 3.24-3.24c1.789 0 3.24 1.45 3.24 3.24s-1.45 3.24-3.24 3.24z"></path>
			</svg>
		</button>
	</div>
</template>

<script>
export default {
	name: "MainContent",
	props: {
		actor: {
			type: Object,
			required: true
		},
		editMode: Boolean,
		isCurrentTurn: Boolean,
		isDefeated: Boolean
	},
	methods: {
		numberOrNull: function(eventTarget) {
			if (eventTarget === null || isNaN(parseInt(eventTarget))) 
			{
				return null;
			}
			return eventTarget;
		},
		updateActorValue: function(key, value) {
			this.$emit("updateActor", key, value);
		},
		onEnterKeyUp: function() {
			if (!this.editMode) {
				return;
			}
			this.nextEmptyRequiredField();
		},
		nextEmptyRequiredField: function() {
			if (this.actor.init === null) {
				return;
			}
			else if (this.actor.name === null) {
				return;
			}
		}
	},
	emits: ['updateActor', 'toggleEdit', 'toggleSelected', 'update:selected', 'editComplete', 'delete']
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
		height: 18px;
	}
	.main-content {
		display: flex;
		margin-bottom: 5px;
	}
	.init-counter {
		flex-grow: 0;
		.init-counter-label {
			padding-top: $card-padding;
			padding-bottom: $card-padding;
			padding-right: $card-padding;
			border-right: 2px solid $white;
			min-width: 20px;
			&:hover {
				cursor: pointer;
			}
		}
		.init-counter-edit {
			max-width: 30px;
		}
	}
	.name {
		flex-grow: 1;
		margin-right: 10px;
		min-width: 60px;
		.name-label {
			padding: $card-padding;
			text-align: left;
			padding-left: 15px;
		}
		.name-edit {
			width: 100%;
		}
	}
	.hitpoints {
		flex-grow: 0;
		padding: $card-padding;
	}
	.hp-edit {
		flex-grow: 0;
		max-width: 40px;
	}
	.icon-button {
		flex-grow: 0;
		background-color: $background;
		color: $white;
		border: 2px solid $white;
		border-radius: 6px;
		font-size: 16px;
		font-weight: bold;
		min-width: 34px;
		&:hover {
			background-color: $background-hover;
			cursor: pointer;
		}
		&.done {
			background-color: $black;
		}
		&.destructive {
			background-color: darkred;
		}
		svg {
			height: 20px;
			width: 20px;
			margin-top: 3px;
			fill: $white;
		}
	}

	/* Chrome, Safari, Edge, Opera */
	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	/* Firefox */
	input[type=number] {
		-moz-appearance: textfield;
	}
</style>