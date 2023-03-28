<template>
    <header>
        <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    </header>

    <main>
        <queryBuilder :config="config"></queryBuilder>
    </main>
</template>

<script setup>
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
import queryBuilder from './components/query-builder.vue'

    const config = {
        groupOperators: [
            {
                name: 'and',
                id: 'and'
            },
            {
                name: 'or',
                id: 'or'
            },
        ],
        ruleOperators: [
            {
                name: 'enthällt',
                id: 'contain'
            },
            {
                name: 'enthällt nicht',
                id: 'notContain'
            },
            {
                name: 'ist',
                id: 'is'
            },
            {
                name: 'ist nicht',
                id: 'isNot'
            },
            {
                name: 'startet mit',
                id: 'startsWith'
            },
            {
                name: 'endet mit',
                id: 'endsWith'
            },
            {
                name: 'hat keine Daten',
                id: 'isEmpty'
            },
            {
                name: 'hat Daten',
                id: 'isNotEmpty'
            },
        ],

        rules: [
            {
                name: 'Name',
                id: 'name',
                type: String,
                icon: '#'
            },
            {
                name: 'Hausnummer',
                id: 'housenumber',
                type: Number,
                icon: '©'
            },
            {
                name: 'Farbe',
                id: 'color',
                type: String,
                icon: '⁂'
            },
        ]
    }

</script>

<style scoped>
header {
    line-height: 1.5;
}

.logo {
    display: block;
    margin: 0 auto 2rem;
}
main{
    max-width: 960px;
    margin: 0 auto;
}
</style>
