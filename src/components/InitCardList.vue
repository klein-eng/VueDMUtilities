<template>
	<div>
        <div id="button-row-top">
            <button @click='incrementTurn' id='next-turn-button'>Next Turn</button>
            <div class="justify-right">
                <button @click='deselectAllActors'>Deselect All</button>
                <input type="number" v-model="damageAmount"/>
                <button @click='applyDamage'>Apply</button>
            </div>
        </div>
        <div>
            <InitCard v-for="(a, index) in actors" :key="a.idNumber" 
            v-model:actor="a.actor"
            v-model:selected="a.selected"
            @add-next="addEmptyActor"
            @edit-complete="sortActors"
            @duplicate="showDuplicateModal(a.actor)"
            @delete="deleteActor(a.idNumber)"
            :isCurrentTurn="this.currentTurnIndex === index"
            :preventNewCard="a.preventNewCard"
        />
        </div>
        <div>
            <button @click="deleteAllActors" class="destructive">Delete All</button>
        </div>
        <div>
            <button @click="saveActors">Save Party</button>
            <button @click="loadActors">Load Party</button>
        </div>
        <duplicate-modal v-model:actor="actorToDuplicate" @duplicate="duplicateActor"/>
	</div>
</template>

<script>
import InitCard from './InitCard/InitCard.vue'
import DuplicateModal from './DuplicateModal.vue'

export default {
	name: 'Root',
	components: {
		InitCard,
        DuplicateModal
	},
    data () {
		return {
			nextID: 2,
            actors: [
                {idNumber: 1, actor: {init: null}}
            ],
            damageAmount: null,
            actorToDuplicate: null,
            currentTurnIndex: 0,
		}
	},
    methods: {
        addEmptyActor: function () {
            this.actors.push({idNumber: this.getNextID(), actor: {init: null}, selected: false});
        },
        showDuplicateModal: function (actor) {
            this.actorToDuplicate = actor;
        },
        duplicateActor: function (howManyDups, initModifier=null) {
            let actor = this.actorToDuplicate
            for(let i = 0; i < howManyDups; i++) {
                this.actors.push({ idNumber: this.getNextID(), actor: { ...actor, init: this.calcInit(actor, initModifier)}, preventNewCard: true });
            }
            this.sortActors();
        },
        calcInit(actor, initModifier) {
            console.log(initModifier);
            if (initModifier === null || initModifier === "") {
                return actor.init;
            }
            let roll = (Math.floor(Math.random() * 20) + 1);
            console.log("Rolled " + roll + "+" + initModifier + "=" + (roll+initModifier))
            return roll + initModifier;
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
        deselectAllActors: function () {
            this.actors.forEach(a => a.selected = false);
        },
        deleteAllActors: function () {
            this.actors = [];
            this.addEmptyActor();
        },
        getNextID: function () {
            this.nextID = +this.nextID + 1;
            return this.nextID - 1;
        },
        incrementTurn: function () {
            let maxIndex = 0;
            this.actors.forEach(a => 
            {
                if (a.actor.init != null) {
                    maxIndex++;
                }
            });
            if(maxIndex > 0) {
                this.currentTurnIndex = (this.currentTurnIndex + 1) % maxIndex;
                if (this.currentTurnIndex == 0) {
                    window.scrollTo(0, 0);
                }
            }
        },
        saveActors: function () {
            localStorage.actors = JSON.stringify(this.actors)
        },
        loadActors: function () {
            this.deleteAllActors();
            if(localStorage.actors) {
                let actors = JSON.parse(localStorage.actors)
                actors.map(a => {
                    a.actor.init = null;
                    a.preventNewCard = true;
                    a.idNumber = this.getNextID();
                    });
                actors[actors.length - 1].preventNewCard = false;
                this.actors = actors;
            }
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../sass/variables.scss';
    #button-row-top {
        display: flex;
        position: sticky;
        flex-wrap: wrap-reverse;
        top: 0;
        background-color: $background;
    }
    #next-turn-button {
        padding: 0 10px;
    }

    .justify-right {
        margin-left: auto;
    }
    input {
        background-color: $white;
        border: solid 1px $white;
        font-size: 18px;
        margin-right: 10px;
        margin-left: 10px;
        max-width: 50px;
    }
    button {
        background-color: $primary;
        border: solid 2px $white;
        border-radius: 5px;
        font-size: 18px;
        font-weight: bold;
        margin: 5px;
    }

    .destructive {
        background-color: $destructive;
        color: $white;
    }
</style>


