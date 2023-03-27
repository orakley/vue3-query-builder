<template>
    <draggable
        class="query-builder-rules"
        :list="items"
        :group="{ name: 'g1' }"
        item-key="id"
        v-bind="dragOptions"
        :component-data="{name:'fade'}"
        handle=".handle"
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
function onMove(){
    console.log('bewegung')
}

// const emit = defineEmits('deleteRule')

// const deleteRule = (event) => {
//     emit('onDeleteRule', event)
// }

</script>