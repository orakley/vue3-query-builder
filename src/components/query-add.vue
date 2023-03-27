<template>
    <select v-model="currentQuery.selectedRule" @change="selectRule(currentQuery.selectedRule)"
            class="select">
        <option disabled selected>Add Rule</option>
        <option v-for="(option, index) in config.options" :key="index" :value="option.id">
            <i>{{ option.icon }}</i> &nbsp;{{ option.name }}    
        </option>
        <option :value="666">
            <i>⁕</i> add Advanced
        </option>
    </select>
    <button type="button" @click="addElement()">Add new Rule</button>

</template>
<script setup>
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
const props = defineProps({
    config: {
        type: Object,
    }
})

const currentQuery = reactive ({
    selectedRule: null,
})

function addElement(){
    'add Element'
}
function selectRule(){
    
}

</script>