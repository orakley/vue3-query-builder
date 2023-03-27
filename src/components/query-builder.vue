<template>
    <div class="query-builder">
                
        <nestedQuery :items="currentQuery.items" :calculatedLevel="0" :config="config" @deleteRule="(event) => deleteRule(event)"></nestedQuery>

        <div class="query-builder-rule query-builder--add">
            <div class="query-builder--add-rule">
                <!-- <queryAdd :config="config"></queryAdd> -->
                
                <select v-model="currentQuery.selectedRule" @change="selectRule(currentQuery.selectedRule)"
                        class="select form-control">
                        <option value="null" selected="selected" disabled>
                            &nbsp;Add Rule</option>
                        <option v-for="(option, index) in config.rules" :key="index" :value="option.id">
                            <i>{{ option.icon }}</i> &nbsp;{{ option.name }}    
                        </option>
                        <option :value="666">
                            <i>⁕</i> add Advanced
                        </option>
                </select>
            </div>
        </div>
        <!-- <queryItem  v-for="(item, index) in currentQuery.items" :key="index"></queryItem> -->
        <!-- <ul>
            <li v-for="(item, index) in currentQuery.items" :key="index">
                Item
            </li>
        </ul> -->
        <button type="button" class="btn btn-primary" @click="createParams()">Gib mir Parameter</button>
    {{ currentQuery.params }}
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
    selectedRule: null,
    items: [],
    params: []
})

function addElement(){
    // myQuery.push({
    //     identifier: "txt",
    //     name: "Text Selection",
    //     component: queryInput,
    //     initialValue: ""
    // })
}

function selectRule(_itemId){
    // console.log(findRule(item))
    if(_itemId !== 666){
        currentQuery.items.push(findRule(_itemId))
    } else {
        addGroup()
    }
    currentQuery.selectedRule = null
    
    addChildKey()
}
function addChildKey(){
    currentQuery.items.forEach(element => element.children = [])
}
function findRule(_itemId){
    return props.config.rules.find(rule => rule.id === _itemId)
}
function addGroup(){
    let _group={
        name: 'advanced',
        id: 'advanced',
        icon: '666',
        children: [

        ]
    }
    currentQuery.items.push(_group)
}
function deleteRule(event){
    currentQuery.items.splice(event.index, 1)
}
function createParams(){
    let _params = []

}
</script>