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
                            :config="config" @deleteRule="$emit('deleteRule', $event)">
                </queryItem>
                <nestedQuery   v-if="element.id === 'advanced'" :class="'nested level-' + calculatedLevel + ' ' + element.id" :config="config"
                    :calculatedLevel="newCalculatedLevel" 
                    :items="element.children">
                </nestedQuery>
                <button v-if="element.id == 'advanced'" >
                    Add new Rule
                </button>
            </div>
        </template>
    </draggable>
</template>
<script setup>
import draggable from "vuedraggable";
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
    console.log(event, event.to)

    // return true only on item who is advanced oder 



                           
    // if(event.draggedContext.element?.children?.length >= 1){
    //     return false
    // } else {
    //     return false
    // }

    // console.log(_isAdvanced)

            var _classes = event.to.className.split(' ').map(str => /qb-rules-container/.test(str)).findIndex(i => i === true)
            let _hasAdvanced = event.to.className.split(' ').map(str => /advanced/.test(str)).findIndex(i => i === true) 
            
            console.log(_hasAdvanced, _classes)
            if(_hasAdvanced > 0 || _classes > 0){
                return true
            } else {
                return false
            }

            // console.log(_parentClasses, _parentLevel)

            // var _futureLevel = _parentLevel+1            
            // var _oldLevel = event.draggedContext.element.level
            // var _maxDepth = undefined

            // if(currentStoredAccordion.keepHierarchy == true){
            //     if((_futureLevel !== _oldLevel)){
            //         return false
            //     }
            // } else if(_maxDepth !== undefined){
            //     if((_futureLevel > _maxDepth) || (_hasChildren && _futureLevel >= _hasChildren)){
            //         return false
            //     }
            // }
}

// const emit = defineEmits('deleteRule')

// const deleteRule = (event) => {
//     emit('onDeleteRule', event)
// }

</script>