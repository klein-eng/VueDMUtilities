<template>
	<div>
        <div id="button-row-top">
            <input type="number" v-model="damageAmount" placeholder="Damage amount"/>
            <button @click='applyDamage'>Apply</button>
        </div>
        <div>
            <InitCard v-for="p in participants" :key="p.idNumber" 
            v-model:actor="p.actor"
            v-model:selected="p.selected"
            v-model:curHP="p.curHP"
            :propName="p.name"
            :propMaxHP="p.maxHP"
            :addNext="this.addEmptyParticipant"
            @edit-complete="sortParticipants"
        />
        </div>
        <div>
            <button @click="clearParticipants">Delete All</button>
        </div>
	</div>
</template>

<script>
import InitCard from './InitCard.vue'

export default {
	name: 'Root',
	components: {
		InitCard,
	},
    data () {
		return {
			nextID: 2,
            participants: [
                {idNumber: 1, actor: {}}
            ],
            damageAmount: null,
		}
	},
    methods: {
        addEmptyParticipant: function () {
            this.participants.push({idNumber: this.getNextID(), actor: {}});
        },
        sortParticipants: function () {
            this.participants.sort((a, b) => {
                return b.actor.init - a.actor.init;
            });
        },
        applyDamage: function () {
            this.participants.forEach(p => {
                if (p.selected && p.actor.curHP !== undefined) {
                    let newHP = p.actor.curHP - this.damageAmount;
                    p.actor.curHP = newHP > 0 ? newHP : 0;
                }
            });
        },
        clearParticipants: function () {
            this.participants = [];
            this.addEmptyParticipant();
        },
        getNextID: function () {
            this.nextID = +this.nextID + 1;
            return this.nextID - 1;
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../sass/variables.scss';
    #button-row-top {
        padding-bottom: 5px;
        display: flex;
        justify-content: right;
    }
    input {
        background-color: $white;
        border: solid 1px $white;
        font-size: 18px;
        margin-right: 10px;
    }
    button {
        background-color: $primary;
        border: solid 2px $white;
        border-radius: 5px;
        font-size: 18px;
        font-weight: bold;
    }
</style>


