<template>
    <div class="qb-actions__add-rule">        
        <select v-model="selectedRule" @change="$emit('selectRule', {selectedRule, calculatedLevel, index}); resetValue()"
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
