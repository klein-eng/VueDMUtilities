<template>
    <div class="init-card-root" :class="{selected: selected, PC: this.actor.IsPC}">
		<main-content
			:actor="this.actor"
			@update-actor="updateActorValue"
			:editMode="this.editMode"
			:isCurrentTurn="this.isCurrentTurn"
			@toggle-edit="this.toggleEdit"
			@toggleSelected="this.$emit('update:selected', !this.selected)"
			:isDefeated="this.actor.curHP <= 0"
			@delete="this.$emit('delete')"
		/>
		<expanded-content
			:actor="this.actor"
			@update-actor="updateActorValue"
			:editMode="editMode"
			:hidden="this.expanded !== 'expanded'"
			@delete="this.$emit('delete')"
			@duplicate="this.$emit('duplicate')"
		/>
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
import MainContent from './MainContent.vue'
import ExpandedContent from './ExpandedContent.vue'

export default {
    name: 'InitCard',
	components: {
		MainContent,
		ExpandedContent
	},
    props: {
        actor: {
			type: Object,
			required: true
		},
		selected: Boolean,
		isCurrentTurn: Boolean,
		preventNewCard: Boolean
    },
	setup: function (props, { emit }) {
		if (props.actor.maxHP && !props.actor.curHP) {
			emit('update:actor', {...props.actor, ["curHP"]: props.actor.maxHP});
		}
	},
	data () {
		return {
			editMode: !(this.hasData()),
			addWhenSaved: (!(this.hasData()) && (!this.preventNewCard === true)),
			expanded: "default"
		}
	},
	methods: {
		hasData: function () {
			return (this.actor.name != null && this.actor.init !== null);
		},
		toggleEdit: function () {
			if (this.editMode === false) {
				this.editMode = true;
			}
			else if (this.hasData()) {
				if (this.actor.curHP === undefined) {
					this.updateActorValue("curHP", this.actor.maxHP);
				}
				this.$emit('editComplete')
				if (this.addWhenSaved) {
					this.addWhenSaved = false;
					this.$emit("addNext");
				}
				this.editMode = false;
			}
		},
		onExpandButtonClicked: function() {
			if (this.expanded === "default") {
				this.expanded = "expanded";
			}
			else {
				this.expanded = "default";
			}
		},
		updateActorValue: function(key, value) {
			this.$emit("update:actor", { ...this.actor, [key]: value });
		}
	},
	emits: ['update:actor', 'update:selected', 'editComplete', 'delete', 'duplicate', 'addNext']
}
</script>

<style lang="scss" scoped>
	@import '../../sass/variables.scss';
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
		&.PC {
			background-color: $PC-background;
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
</style>