<template>
    <select v-model="value" 
            @change="$emit('emitInput', {value})" 
            class="form-control">
            <option value="null">Select Value</option>
            <option v-for="option in options.select"
                :value="option" :key="option.id">
                {{ option.name }}    
            </option>
    </select>
</template>
<script setup>
import { shallowRef,  ref, computed } from 'vue'

const value = shallowRef(null)
const options = shallowRef( {
    select: [
        { 
            name: 'test 1',
            id: 1
        },
        { 
            name: 'test 2',
            id: 2
        },
        { 
            name: 'test 3',
            id: 3
        },
        { 
            name: 'test 4',
            id: 4
        },
        { 
            name: 'test 5',
            id: 5
        },
        { 
            name: 'test 6',
            id: 6
        },
        { 
            name: 'test 7',
            id: 7
        },
        { 
            name: 'test 8',
            id: 8
        },
    ]})
</script>