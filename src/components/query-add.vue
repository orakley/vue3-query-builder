<template>
    <div class="qb-actions__add-rule">        
        <select v-model="selectedRule" @change="$emit('addRule', {selectedRule, calculatedLevel, index, parentuuid}); resetValue()"
                class="select form-control">
                <option value="null" selected="selected" disabled>
                    &nbsp;Add Rule</option>
                <option v-for="(option, index) in config.rules" :key="index" :value="option.identificator">
                    <i>{{ option.icon }}</i> &nbsp;{{ option.name }}    
                </option>
                <option v-if="calculatedLevel < 2"
                        :value="'group'">
                    <i>+</i> add group
                </option>
        </select>
        {{ calculatedLevel }}
    </div>

</template>
<script setup>
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
const props = defineProps({
    config: {
        type: Object,
    },
    calculatedLevel: {
        type: Number
    },
    index: {
        type: Number,
    },
    parentuuid: {
        type: String
    }
})

const selectedRule = ref('')

// const currentQuery = reactive ({
//     selectedRule: null,
// })

function addElement(){
    'add Element'
}
function resetValue(){
    selectedRule.value = null
}
// function selectRule(){
    
// }

</script>
