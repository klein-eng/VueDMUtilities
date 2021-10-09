<template>
	<div>
        <div id="button-row-top">
            <input type="number" v-model="damageAmount" placeholder="Damage amount"/>
            <button @click='applyDamage'>Apply</button>
        </div>
        <div>
            <InitCard v-for="a in actors" :key="a.idNumber" 
            v-model:actor="a.actor"
            v-model:selected="a.selected"
            @add-next="addEmptyActor"
            @edit-complete="sortActors"
            @duplicate="duplicateActor(a.actor, 1)"
            @delete="deleteActor(a.idNumber)"
        />
        </div>
        <div>
            <button @click="deleteAllActors">Delete All</button>
        </div>
	</div>
</template>

<script>
import InitCard from './InitCard/InitCard.vue'

export default {
	name: 'Root',
	components: {
		InitCard,
	},
    data () {
		return {
			nextID: 2,
            actors: [
                {idNumber: 1, actor: {init: null}}
            ],
            damageAmount: null,
		}
	},
    methods: {
        addEmptyActor: function () {
            this.actors.push({idNumber: this.getNextID(), actor: {init: null}});
        },
        duplicateActor: function (actor, howManyDups, initModifier=null) {
            for(let i = 0; i < howManyDups; i++) {
                this.actors.push({ idNumber: this.getNextID(), actor: { ...actor, init: this.calcInit(actor, initModifier) } });
            }
            this.sortActors();
        },
        calcInit(actor, initModifier) {
            if (initModifier === null) {
                return actor.init;
            }
            return (Math.floor(Math.random() * 20) + 1) + initModifier;
        },
        deleteActor: function (actorID) {
            let index = this.actors.findIndex(a => a.idNumber === actorID);
            if (index > -1) {
                this.actors.splice(index, 1)
            }
        },
        sortActors: function () {
            this.actors.sort((a, b) => {
                if (b.actor.init < 0 && a.actor.init === null) {
                    return 1;
                }
                if (a.actor.init < 0 && b.actor.init === null) {
                    return -1;
                }
                return b.actor.init - a.actor.init;
            });
        },
        applyDamage: function () {
            this.actors.forEach(a => {
                if (a.selected && a.actor.curHP !== undefined) {
                    let newHP = a.actor.curHP - this.damageAmount;
                    a.actor.curHP = newHP > 0 ? newHP : 0;
                }
            });
        },
        deleteAllActors: function () {
            this.actors = [];
            this.addEmptyActor();
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


