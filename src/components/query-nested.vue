<template>
    <draggable
        class="qb-draggable qb-rules-container"
        :list="items"
        :group="{ name: 'g1' }"
        item-key="id"
        v-bind="dragOptions"
        :component-data="{name:'fade'}"
        handle=".qb-rule-handle"
        :move="onMove"
    >
        <template #item="{ element, index}">
            <div class="qb-item" :class="_isGroup(element)" :data-id="element.identificator">
                <queryItem  :rule="element" :index="index" 
                            :calculatedLevel="calculatedLevel"
                            :currentQuery="currentQuery"
                            :config="config" 
                            :parentuuid="parentuuid"
                            @deleteRule="$emit('deleteRule', $event)"
                            @updateRule="$emit('updateRule', $event)"
                            @levelOperatorValue="$emit('levelOperatorValue', $event)">
                </queryItem>
                
                <nestedQuery    v-if="element.isGroup" 
                                :class="'nested group level-' + calculatedLevel + ' ' + element.identificator" 
                                :config="config"
                                :parentuuid="element.uuid"
                                :calculatedLevel="newCalculatedLevel" 
                                @deleteRule="$emit('deleteRule', $event)"
                                @updateRule="$emit('updateRule', $event)"
                                @addRule="$emit('addRule', $event)"
                                @levelOperatorValue="$emit('levelOperatorValue', $event)"
                                :items="element.children">
                </nestedQuery>

                <queryAdd   v-if="element.children" 
                            :index="index"
                            :calculatedLevel="newCalculatedLevel"
                            :config="config" 
                            :parentuuid="element.uuid"
                            @addRule="$emit('addRule', $event)"/>
            </div>
        </template>
    </draggable>
</template>
<script setup>
import draggable from "vuedraggable";
import queryAdd from "./query-add.vue"
import queryItem from "./query-item.vue"
import nestedQuery from "./query-nested.vue"
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';

const props = defineProps({
    items: {
        type: Array
    },
    calculatedLevel: {
        type: Number
    },
    config:{
        type: Object,
    },
    currentQuery: {
        type: Object
    },
    parentuuid: {
        type: String
    }
})



const _isGroup = (element) => {if(element.isGroup){return 'qb-item__group ' + element.identificator}}

const newCalculatedLevel = computed(() => props.calculatedLevel+1)

const dragOptions = reactive({
    animation: 200,
    group: "query",
    disabled: false,
    ghostClass: "ghost",
    dataIdAttr: 'data-id', 
})
function onMove(event){

    var _classes = event.to.className.split(' ').map(str => /qb-rules-container/.test(str)).findIndex(i => i === true)
    let _isGroup = event.to.className.split(' ').map(str => /group/.test(str)).findIndex(i => i === true) 
    
    if(_isGroup > 0 || _classes > 0){
        return true
    } else {
        return false
    }
}


</script>