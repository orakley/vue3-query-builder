<template>
    <div class="query-builder-rule" :class="'level-' + calculatedLevel + ' query-builder-rule--' + rule.id">
        <div class="rule-group-operator">
            <select class="form-control" v-if="index !== 0">
                <option v-for="operator in config.groupOperators" :key="operator.id">
                    {{ operator.name }}    
                </option>
            </select>
        </div>
        <div class="rule">
            <div class="rule-handle">        
            </div>
            <div class="rule-name">
                <div class="icon">
                    {{ rule.icon }}
                </div>
                <div class="name">
                    {{ rule.name }}
                </div>
            </div>
            <div v-if="config.ruleOperators" class="rule-operators">
                <select class="form-control">
                    <option v-for="(operator, index) in config.ruleOperators" :key="index">
                        {{ operator.name}}
                    </option>
                </select>
            </div>
            <div class="rule-input">
                <input class="input-field" v-model="rule.value" type="text">
            </div>        
            <div class="rule-actions">
                <button type="button" @click="$emit('deleteRule', {rule, index})"
                        class="btn btn-secondary btn-icon">
                        <img src="../assets/delete.svg">
                </button>
            <!-- Add to Advanced Filter -->
            </div>
        </div>
    </div>
</template>
<script setup>
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
const props = defineProps({
    rule: {
        type: Object
    },
    index: {
        type: Number,
    },
    calculatedLevel: {
        type: Number
    },
    config:{
        type: Object,
    }
})
// const emit = defineEmits('deleteRule')

// const deleteRule = (event) => {
//     emit('onDeleteRule', props.rule)
// }
</script>
