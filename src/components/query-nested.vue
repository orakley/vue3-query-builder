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
            <div class="qb-item" :class="_isAdvanced(element)" :data-id="element.id">

                <queryItem  :rule="element" :index="index" :calculatedLevel="calculatedLevel"
                            :currentQuery="currentQuery"
                            :config="config" 
                            @deleteRule="$emit('deleteRule', $event)"
                            @updateRule="$emit('updateRule', $event)"
                            @levelOperatorValue="$emit('levelOperatorValue', $event)">
                </queryItem>
                
                <nestedQuery    v-if="element.id === 'advanced'" 
                                :class="'nested level-' + calculatedLevel + ' ' + element.id" 
                                :config="config"
                                :calculatedLevel="newCalculatedLevel" 
                                @deleteRule="$emit('deleteRule', $event)"
                                @updateRule="$emit('updateRule', $event)"
                                @levelOperatorValue="$emit('levelOperatorValue', $event)"
                                :items="element.children">
                </nestedQuery>

                <queryAdd   v-if="element.id == 'advanced'" 
                            :index="index"
                            :calculatedLevel="calculatedLevel"
                            :config="config" @selectRule="$emit('selectRule', $event)"/>

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
    }
})

// const emit = defineEmit({

// })

const _isAdvanced = (element) => {if(element.id === 'advanced'){return 'qb-item__' + element.id}}

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
    let _hasAdvanced = event.to.className.split(' ').map(str => /advanced/.test(str)).findIndex(i => i === true) 
    
    if(_hasAdvanced > 0 || _classes > 0){
        return true
    } else {
        return false
    }
}


</script>