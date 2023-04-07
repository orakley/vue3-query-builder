<template>
    <div class="qb-rule" :class="'qb-rule--' + rule.identificator + ' level-' + calculatedLevel">
        <div class="qb-rule-operator">
            <select v-if="showOperator"
                    v-model="currentRule.levelOperator" 
                    @change="$emit('levelOperatorValue', {currentRule, index, calculatedLevel})" 
                    class="form-control">
                    <option v-for="operator in config.levelOperators"
                        :selected="operator.identificator == 'AND' ? 'selected' : ''"
                        :value="operator.identificator" :key="operator.identificator">
                        {{ operator.name }}    
                    </option>
            </select>
            <small v-if="!showOperator">    
                {{ levelOperators[calculatedLevel].name }}
            </small>
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
                        @change="ruleUpdate()">

                    <option v-for="operator in config.ruleOperators" :value="operator.identificator" :key="operator.identificator">
                        {{ operator.name}}
                    </option>
                </select>
            </div>
            <div class="qb-rule-input">
                <input v-if="!rule.component" class="input-field" v-model="currentRule.value" :type="rule.type" 
                :placeholder="rule.placeholder"
                @input="ruleUpdate()">
                <component v-else :is="{...rawComponent}" @emitInput="(event) => updateInput(event)">
                </component>
            </div>        
            <div class="qb-rule-actions">
                <button type="button" @click="$emit('deleteRule', {rule, index, calculatedLevel, parentuuid})"
                        class="btn btn-secondary btn-icon">
                        <img src="../assets/delete.svg">
                </button>
            </div>
        </div>
    </div>
</template>
<script setup>
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs, markRaw } from 'vue';

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
    parentIndex: {
        type: Number
    },
    parentuuid: {
        type: String
    },
    levelOperators: {
        type: Array
    }

})
const emit = defineEmits(['updateRule', 'deleteRule', 'levelOperatorValue', 'emitInput'])

const rawComponent = markRaw(props.rule.component)


watch(()=> props, (current, prev) => {
    ruleUpdate()
    if(current.index){
        currentRule.index = props.index
    }
}, { deep: true });

const currentRule = reactive({
    levelOperator: 'AND',
    uuid: props.rule.uuid,
    value: props.value,
    operator: 'contain',
    level: props.calculatedLevel,
    index: props.index,
})

const showOperator = computed(() => {
    const   _isFirstofGroup = props.index == 0 && props.rule.isGroup == true,
            _isSecondofGroup = props.index == 0 && props.rule.level > 0,
            _isFirst = props.index == 0 && props.rule.level == 0,
            _isSecond = props.index == 1 && props.rule.level == 0;
            
    if(_isFirst){
        return false
    }
    if(_isSecondofGroup || _isSecond || _isFirstofGroup ){
        return true
    }

})

onMounted(() =>{
    currentRule.value = props.rule.initialValue || ''

    if(props.parentuuid){
        currentRule.parentuuid = props.parentuuid
    }
    if(props.rule.isGroup){
        currentRule.index = props.index
    }
    ruleUpdate()
})


function ruleUpdate(){
    emit('updateRule', {currentRule, props}); 
}

function updateInput(event){
    console.log('input', event)
    currentRule.value = event.value
    ruleUpdate()
}

// Vue.createApp({
//   components: {
//     'my-component': Vue.defineAsyncComponent(() => loadModule('./myComponent.vue', opts))
//   },
//   ...
</script>
