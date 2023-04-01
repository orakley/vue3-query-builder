<template>
    <div class="qb-container">
                
        <nestedQuery    :items="currentQuery.rules" 
                        :calculatedLevel="0" :config="config" 
                        :currentQuery="currentQuery"
                        @deleteRule="(event) => deleteRule(event)"
                        @updateRule="(event) => updateRule(event)"
                        @levelOperatorValue="(event) => setLevelOperator(event)"
                        @selectRule="(event) => selectRule(event)">
        </nestedQuery>

        <div class="qb-actions">
            <queryAdd   :config="config" 
                        @selectRule="(item) => selectRule(item)"/>

            <!-- <button v-if="currentQuery.query" type="button" 
                    class="btn btn-primary qb-actions__search" 
                    @click="createParams()">
                    Gib Parameter
            </button> -->
        </div>


        <pre v-if="currentQuery">
            {{ currentQuery }}
        </pre>

        <!---<query-builder :config="config" v-model="query">
             <template #groupOperator="props">
                <div class="query-builder-group-slot__group-selection form-select">
                    <select class="form-select btn btn-white w-auto text-left"
                        :value="props.currentOperator"
                        @input="props.updateCurrentOperator($event.target.value)">
                        <option disabled value>Operator auswählen...</option>
                        <option
                        v-for="operator in props.operators"
                        :key="operator.identifier"
                        :value="operator.identifier"
                        v-text="operator.name"
                        />
                    </select>
                </div>
            </template>

            <template #groupControl="props">
                <group-ctrl-slot :group-ctrl="props"/>
            </template>

            <template #rule="props">
                <rule-slot :rules="config.rules" :ruleCtrl="props" :query="query" :operator="query"/>
            </template>

        </query-builder> -->


    </div>
</template>
<script setup>
// import queryItem from "./query-item.vue"
import nestedQuery from "./query-nested.vue"
import queryAdd from "./query-add.vue"
// import queryInput from "./elements/input.vue"
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';

const props = defineProps({
    config: {
        type: Object,
    },
    cssModifier: {
        type: String,
        default: 'item'
    }
})
// const emit = defineEmits(['addRule'])

const currentQuery = reactive ({
    // selectedRule: null,
    levelOperator: {
        name: "and",
        id: "AND" 
    },
    // items: [],
    // query: {
        operator: '',
        level: 0,
        rules: [],
    // }
})

function selectRule(event){
    console.log('select', event)

    
        if(event.calculatedLevel !== undefined || event.calculatedLevel === 1){
            
                currentQuery.rules[event.index].children.push(findRule(event.selectedRule))
                // console.log(currentQuery.rules[event.index], currentQuery.rules[event.index].children)

        } else  {
            let _find = findRule(event.selectedRule)

            let _addOn = {
                index: currentQuery.rules.length,
                value: '',
                operator: 'contain',
                level: 0
            }
            let _merged = {..._addOn, ..._find}
            currentQuery.rules.push(_merged)
        }

    // currentQuery.selectedRule = 0
}
function findRule(_id){
    console.log(_id)
    if(_id === 666 ){
        return {
            name: 'advanced',
            id: 'advanced',
            icon: '666',
            children: []
        }
    } else {
        return props.config.rules.find(rule => rule.id === _id)
    }
}
function filterRule(_id, _level, _index){
    return currentQuery.rules.filter(item => {
        return (item['level'] == _level) && (item['id'] == _id) && (item['index'] == _index) 
    })

}
// function addGroup(){
//     let _group={
//         name: 'advanced',
//         id: 'advanced',
//         icon: '666',
//         children: []
//     }
//     currentQuery.rules.push(_group)
// }
function deleteRule(event){
    if(event.calculatedLevel == 0){
        currentQuery.rules.splice(event.index, 1)
    } else {
        console.log('delete', event)
    }
}
function createParams(){
    let _params = []

}
function setLevelOperator(event){
    console.log('levelOper', event)


    currentQuery.levelOperator = props.config.levelOperators.find(operator => operator.id == event.currentRule.levelOperator)

}

function updateRule(event){
    let _find = filterRule(event.currentRule.id, event.currentRule.level, event.currentRule.index)
    console.log('found:', _find)

    currentQuery.rules[event.currentRule.index].value = event.currentRule.value
    currentQuery.rules[event.currentRule.index].operator = event.currentRule.operator

    // _find.value = event.currentRule.value,
    // _find.operator = event.currentRule.operator

}


// q[operatorIdentifier]:AND
// q[children][0][identifier]:fulltext
// q[children][0][value]:Sachsen
// q[children][1][operatorIdentifier]:OR
// q[children][1][children][0][identifier]:fulltext
// q[children][1][children][0][value]:Brücke
// q[children][1][children][1][identifier]:fulltext
// q[children][1][children][1][value]:Tunnel
// q[children][1][children][2][operatorIdentifier]:AND_NOT
// q[children][1][children][2][children][0][identifier]:fulltext
// q[children][1][children][2][children][0][value]:Sachsen-Anhalt
</script>