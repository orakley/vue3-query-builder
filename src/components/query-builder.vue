<template>
    <div class="qb-container">
                
        <nestedQuery    :items="currentQuery.rules" 
                        :calculatedLevel="calculatedLevel" 
                        :config="config" 
                        :levelOperators="currentQuery.levelOperators"
                        @deleteRule="(event) => deleteRule(event)"
                        @updateRule="(event) => updateRule(event)"
                        @levelOperatorValue="(event) => setLevelOperator(event)"
                        @addRule="(event) => addRule(event)">
        </nestedQuery>

        <div class="qb-actions">
            <queryAdd   :config="config" 
                        :calculatedLevel="calculatedLevel" 
                        @addRule="(item) => addRule(item)"/>

            <!-- <button v-if="currentQuery.rules.length" type="button" 
                    class="btn btn-primary qb-actions__search" 
                    @click="createParams()">
                    Gib Parameter
            </button> -->
            <button v-if="currentQuery.rules.length" type="button" 
                    class="btn btn-primary qb-actions__delete" 
                    @click="currentQuery.rules = []">
                    Filter löschen
            </button>
        </div>


        <pre v-if="currentQuery">
            {{ currentQuery }}
        </pre>

    </div>
</template>
<script setup>
    import nestedQuery from "./query-nested.vue"
    import queryAdd from "./query-add.vue"
    import { reactive } from 'vue';

    const props = defineProps({
        config: {
            type: Object,
        },
        cssModifier: {
            type: String,
            default: 'item'
        }
    })
    const currentQuery = reactive ({
        levelOperators: [],
        operator: '',
        level: 0,
        rules: [],
    })
    const calculatedLevel = 0

    function getUUID(){
        // https://stackoverflow.com/questions/105034/how-do-i-create-a-guid-uuid
        return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
            (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
        );
    }

    function findRule(obj, _uuid){
        for(const i in obj){
            if(obj[i].uuid == _uuid) return obj[i]
            if(obj[i].children){
                const nestedObj = findRule(obj[i].children, _uuid)
                if (nestedObj) return nestedObj;
            }
        }
    }

    function selectRule(_id, _event){
        if(_id === 'group'){
            let _groupElements = {
                uuid: getUUID(),
                type: {
                    identificator: 'group',
                    icon: '+',
                },
                isGroup: true,
                level: _event.calculatedLevel || 0,
                children: []
            }
            return {..._groupElements, ...props.config.rules[0]}
        } else {
            return props.config.rules.find(rule => rule.identificator === _id)
        }
    }
        
    function addRule(event){
        let _add = {
            index: !event.calculatedLevel ? currentQuery.rules.length : 0,
            value: '',
            operator: 'contain',
            level: !event.calculatedLevel ? 0 : event.calculatedLevel,
            uuid: getUUID(),
            type: selectRule(event.selectedRule, event)
        }
        if(event.parentuuid){        
            findRule(currentQuery.rules, event.parentuuid).children.push(_add)
        } else  {
            currentQuery.rules.push(_add)
        }
        if(event.calculatedLevel >= currentQuery.levelOperators.length){
            addLevelOperator(event.calculatedLevel)
        }
    }

    function deleteRule(event){
        let _findItem = findRule(currentQuery.rules, event.parentuuid ? event.parentuuid : event.rule.uuid)
        if(event.parentuuid){
            _findItem.children.splice(_findItem.children.findIndex(item => item.uuid == event.rule.uuid), 1)
        } else {
            currentQuery.rules.splice(event.index, 1)
        }
        if(event.rule.isGroup){
            deleteLevelOperator(event.rule.level + 1)
        }
    }

    function createParams(){
        let _params = []
    }

    function addLevelOperator(level){
        let _level = {
            level: level,
        }
        let _findOperator = {...props.config.levelOperators[0], ..._level}
        currentQuery.levelOperators.push(_findOperator)
    }
    
    function deleteLevelOperator(level){
       currentQuery.levelOperators.splice(currentQuery.levelOperators.findIndex(operator => operator.level === level), 1)
    }

    function setLevelOperator(event){
        let _findOperator = props.config.levelOperators.find(operator => operator.identificator == event.currentRule.levelOperator)

        let _findCurrentOperator = currentQuery.levelOperators.find(operator => operator.level === event.calculatedLevel)
        _findCurrentOperator.identificator = _findOperator.identificator
        _findCurrentOperator.name = _findOperator.name
    }

    function updateRule(event){
        let _find = findRule(currentQuery.rules, event.currentRule.uuid)
        if(event._type === 'resetValue'){
            _find.value = null
        } else {
            _find.value = event.currentRule.value
        }

        _find.operator = event.currentRule.operator
        _find.index = event.props.index
        _find.level = event.props.calculatedLevel
        _find.type = event.currentRule.type
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