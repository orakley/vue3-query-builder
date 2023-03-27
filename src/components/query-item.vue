<template>
    <div class="query-builder-rule" :class="'level-' + calculatedLevel">
        <div class="handle">        
        </div>
        <div class="name">
            {{ rule.name }}
        </div>
        <div v-if="config.ruleOperators" class="operators">
            <select class="form-control">
                <option v-for="(operator, index) in config.ruleOperators" :key="index">
                    {{ operator.name}}
                </option>
            </select>
        </div>
        <div class="input">
            <input class="input-field" v-model="rule.value" type="text">
        </div>        
        <div class="actions">
            <button type="button" @click="$emit('deleteRule', {rule, index})"
                    class="btn btn-secondary btn-icon">
                    delete
            </button>
            <!-- Add to Advanced Filter -->
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
