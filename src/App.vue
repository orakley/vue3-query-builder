<template>
    <header>
        <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    </header>

    <main>
        <queryBuilder :config="config"></queryBuilder>
    </main>
</template>

<script setup>
import colorInput from "./components/rules/color.vue"

import selectInput from "./components/rules/select.vue"
import textInput from "./components/rules/text.vue"
import numberInput from "./components/rules/number.vue"
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
import queryBuilder from './components/query-builder.vue'

    const config = {
        levelOperators: [
            {
                name: 'and',
                identificator: 'AND'
            },
            {
                name: 'or',
                identificator: 'OR'
            },
        ],
        ruleOperators: [
            {
                name: 'enthällt',
                identificator: 'contain'
            },
            {
                name: 'enthällt nicht',
                identificator: 'notContain'
            },
            {
                name: 'ist',
                identificator: 'is'
            },
            {
                name: 'ist nicht',
                identificator: 'isNot'
            },
            {
                name: 'startet mit',
                identificator: 'startsWith'
            },
            {
                name: 'endet mit',
                identificator: 'endsWith'
            },
            {
                name: 'hat keine Daten',
                identificator: 'isEmpty'
            },
            {
                name: 'hat Daten',
                identificator: 'isNotEmpty'
            },
        ],

        rules: [
            {
                name: 'Name',
                identificator: 'name',
                type: "text",
                icon: '#',
                component: textInput,
                initialValue: "",
                placeholder: "TEXT - Placeholder",
            },
            {
                name: 'Hausnummer',
                identificator: 'housenumber',
                type: 'Number',
                icon: '©',
                // component: numberInput,
                initialValue: "",
                placeholder: 'Hausnummer'
            },
            {
                name: 'Farbe',
                identificator: 'color',
                type: String,
                icon: '⁂',
                // component: colorInput,
                initialValue: "#333",
                placeholder: 'Farbe'
            },
            {
                name: "Selection",
                identificator: "txt",
                component: selectInput,
                initialValue: "",
                placeholder: "Placeholder"
            },
        ]
            //         {
            //             identifier: 'publish_start_to',
            //             name: 'Veröffentlichungsdatum bis',
            //             component: publishstartto,
            //             initialValue: () => new Date().toLocaleDateString('en-CA'),
            //         },
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
