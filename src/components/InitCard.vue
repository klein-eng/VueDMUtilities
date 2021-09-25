<template>
    <div class="init-card-root" :class="{selected: selected}">
		<div class="main-content">
			<div class="init-counter">
				<input v-if="editMode" :value="init" type='number' @input="$emit('update:init', this.numberOrNull($event.target.value))" placeholder="Init" class="init-counter-edit"/>
				<div v-else @click="$emit('update:selected', !this.selected)" class="init-counter-label">{{ init }}</div>
			</div>
			<div class="name">
				<input v-if="editMode" v-model="name" placeholder="Name" class="name-edit"/>
				<div v-else class="name-label">{{ name }}</div>
			</div> 
			<div v-if="maxHP && !editMode" class="hitpoints">{{curHP}} / {{maxHP}}</div>
			<input v-else-if="editMode" v-model.number="this.maxHP" class="hp-edit" placeholder="HP" />
			<button v-if="editMode" @click="toggleEdit" class="edit-button done">
				<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 16 16">
					<path d="M13.5 2l-7.5 7.5-3.5-3.5-2.5 2.5 6 6 10-10z"></path>
				</svg>
			</button>
			<button v-else @click="toggleEdit" class="edit-button">
				<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 16 16">
					<path d="M14.59 9.535c-0.839-1.454-0.335-3.317 1.127-4.164l-1.572-2.723c-0.449 0.263-0.972 0.414-1.529 0.414-1.68 0-3.042-1.371-3.042-3.062h-3.145c0.004 0.522-0.126 1.051-0.406 1.535-0.839 1.454-2.706 1.948-4.17 1.106l-1.572 2.723c0.453 0.257 0.845 0.634 1.123 1.117 0.838 1.452 0.336 3.311-1.12 4.16l1.572 2.723c0.448-0.261 0.967-0.41 1.522-0.41 1.675 0 3.033 1.362 3.042 3.046h3.145c-0.001-0.517 0.129-1.040 0.406-1.519 0.838-1.452 2.7-1.947 4.163-1.11l1.572-2.723c-0.45-0.257-0.839-0.633-1.116-1.113zM8 11.24c-1.789 0-3.24-1.45-3.24-3.24s1.45-3.24 3.24-3.24c1.789 0 3.24 1.45 3.24 3.24s-1.45 3.24-3.24 3.24z"></path>
				</svg>
			</button>
		</div>
		<div class='expanded-content' :style='{display: getExpandedContentDisplay()}'>
			<div>
				<div>AC: 15</div>
			</div>
		</div>
		<button @click="onExpandButtonClicked" class="more-info-button">
			<svg :class='{expanded: expanded==="expanded"}' version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="306px" height="306px" viewBox="0 0 306 306" style="enable-background:new 0 0 306 306;" xml:space="preserve">
				<g id="expand-less">
					<polygon points="153,58.65 0,211.65 35.7,247.35 153,130.05 270.3,247.35 306,211.65 		"/>
				</g>
			</svg>
		</button>
	</div>
</template>

<script>
export default {
    name: 'InitCard',
    props: {
        init: Number,
		selected: Boolean,
		curHP: Number,
		propName: String,
		propMaxHP: Number,
		addNext: Function,
    },
	setup: function (props, { emit }) {
		if (props.propMaxHP && !props.curHP) {
			emit('update:curHP', props.propMaxHP);
		}
	},
	data () {
		return {
			name: this.propName ? this.propName : null,
			maxHP: this.propMaxHP ? this.propMaxHP : null,
			editMode: !(this.propName && this.init),
			addWhenSaved: !(this.propName && this.init),
			expanded: "default"
		}
	},
	methods: {
		hasData: function () {
			return (this.name && this.init !== null);
		},
		toggleEdit: function () {
			if (this.editMode === false) {
				this.editMode = true;
			}
			else if (this.hasData()) {
				if (!this.curHP) {
					this.$emit('update:curHP', this.maxHP);
				}
				this.$emit('editComplete')
				if (this.addWhenSaved) {
					this.addWhenSaved = false;
					this.addNext();
				}
				this.editMode = false;
			}
		},
		getFlexOrder: function () {
			if (!this.init) {
				return 1;
			}
			return this.init * -1;
		},
		numberOrNull: function(eventTarget) {
			if (eventTarget === null || isNaN(parseInt(eventTarget))) 
			{
				console.log("stahp")
				return null;
			}
			return eventTarget;
		},
		getExpandedContentDisplay: function() {
			if (this.expanded === "expanded") {
				return "block";
			}
			else {
				return "none";
			}
		},
		onExpandButtonClicked: function() {
			if (this.expanded === "default") {
				this.expanded = "expanded";
			}
			else {
				this.expanded = "default";
			}
		}
	},
	emits: ['update:init', 'update:curHP', 'update:selected', 'editComplete']
}
</script>

<style lang="scss" scoped>
	@import '../sass/variables.scss';
	.init-card-root {
		background-color: $primary;
		color: $black;
		padding: $card-padding;
		padding-bottom: 0;
		border: 4px solid $white;
		&.selected {
			border-color: turquoise;
		}
		border-radius: 10px;
		font-size: 16px;
		font-weight: bold;
		margin-bottom: 5px;
		.main-content {
			display: flex;
		}
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
	.edit-button {
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
		svg {
			height: 20px;
			width: 20px;
			margin-top: 3px;
			fill: $white;
		}
	}
	.more-info-button {
		width: calc(100% + 2 * (#{$card-padding}));
		margin-left: -$card-padding;
		background: $background;
		height: 17px;
		border: none;
		border-bottom-left-radius: 5px;
		border-bottom-right-radius: 5px;
		margin-top: 5px;
		svg {
			height: 12px;
			width: 12px;
			fill: $white;
			transform: scaleY(-1);
			margin-bottom: -2px;
			&.expanded {
				transform: none;
			}
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