<template>
    <draggable
        class="query-builder-rules"
        :list="items"
        :group="{ name: 'g1' }"
        item-key="id"
        v-bind="dragOptions"
        :component-data="{name:'fade'}"
        handle=".rule-handle"
        :move="onMove"
    >
        <template #item="{ element, index}">
            <div  :data-id="element.id">
                <queryItem  :rule="element" :index="index" :calculatedLevel="calculatedLevel"
                            :config="config" @deleteRule="$emit('deleteRule', $event)">
                </queryItem>

                <nestedQuery    :class="'nested level-' + calculatedLevel" :config="config"
                                :calculatedLevel="newCalculatedLevel" 
                                :items="element.children">
                </nestedQuery>
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

const newCalculatedLevel = computed(() => props.calculatedLevel+1)

const dragOptions = reactive({
    animation: 200,
    group: "query",
    disabled: false,
    ghostClass: "ghost",
    dataIdAttr: 'data-id', 
})
function onMove(event){
    console.log(event, 'bewegung', event.draggedContext.element?.children)

                           
    // if(event.draggedContext.element?.children?.length >= 1){
    //     return false
    // } else {
    //     return false
    // }

    // console.log(_isAdvanced)

            var _parentClasses = event.to.className.split(' ')
            var _findString = _parentClasses.map(str => /level/.test(str))
            var _parentLevelIndex = _findString.findIndex(i => i === true) 
            if(_parentLevelIndex !== -1){
                var _parentLevel = Number(_parentClasses[_parentLevelIndex].split('').reverse().join('').substring(0,1))
            } else {
                var _parentLevel = -1
            }

            console.log(_parentClasses, _parentLevel)

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