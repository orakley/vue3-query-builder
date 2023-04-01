<template>
    <div class="qb-rule" :class="'qb-rule--' + rule.id + ' level-' + calculatedLevel">
        <div class="qb-rule-operator">
            <select v-if="index === 1 || calculatedLevel > 0"
                    v-model="currentRule.levelOperator" @change="$emit('levelOperatorValue', {currentRule, index, calculatedLevel})" 
                    class="form-control">
                <option v-for="operator in config.levelOperators"
                        :selected="operator.id == 'AND' ? 'selected' : ''"
                        :value="operator.id" :key="operator.id">
                    {{ operator.name }}    
                </option>
            </select>
            <small v-if="index > 1">{{ currentQuery.levelOperator?.name }}</small>
        </div>
        <div class="qb-rule-container">
            <div class="qb-rule-handle">        
            </div>
            <div class="qb-rule-name">
                <div v-if="rule.icon" class="icon">
                    {{ rule.icon }}
                </div>
                <div class="qb-name">
                    {{ rule.name }}
                </div>
            </div>
            <div v-if="config.ruleOperators" class="qb-rule-operators">
                <select v-model="currentRule.operator" class="form-control" 
                        @change="$emit('updateRule', {currentRule}); ruleUpdate()">

                    <option v-for="operator in config.ruleOperators" :value="operator.id" :key="operator.id">
                        {{ operator.name}}
                    </option>
                </select>
            </div>
            <div class="qb-rule-input">
                <!-- Component: <{{rule.component}} /> <br> -->
                <input  class="input-field" v-model="currentRule.value" :type="rule.type" 
                        :placeholder="rule.placeholder"
                        @input="$emit('updateRule', {currentRule}); ruleUpdate()">
            </div>        
            <div class="qb-rule-actions">
                <button type="button" @click="$emit('deleteRule', {rule, index, calculatedLevel})"
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
    },
    currentQuery: {
        type: Object
    }

})
onMounted(() =>{
    currentRule.value = props.rule.initialValue || ''
})

const currentRule = reactive({
    levelOperator: 'AND',
    id: props.rule.id,
    value: '',
    operator: 'contain',
    level: props.calculatedLevel,
    index: props.index,
})

function ruleUpdate(){
}
// onMounted(() => {
//     inputValue = props.rule.value
// })
// const emit = defineEmits('deleteRule')

// const deleteRule = (event) => {
//     emit('onDeleteRule', props.rule)
// }
</script>
