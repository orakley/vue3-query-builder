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
import textInput from "./components/rules/text.vue"
import numberInput from "./components/rules/number.vue"
import { computed, reactive, onMounted, onBeforeMount , ref, watch, watchEffect, toRef, toRefs } from 'vue';
import queryBuilder from './components/query-builder.vue'

    const config = {
        levelOperators: [
            {
                name: 'and',
                id: 'AND'
            },
            {
                name: 'or',
                id: 'OR'
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
                icon: '#',
                // component: textInput,
                initialValue: "",
                placeholder: "TEXTINPUT",
            },
            {
                name: 'Hausnummer',
                id: 'housenumber',
                type: 'Number',
                icon: '©',
                // component: numberInput,
                initialValue: "",
                placeholder: 'Hausnummer'
            },
            {
                name: 'Farbe',
                id: 'color',
                type: String,
                icon: '⁂',
                // component: colorInput,
                initialValue: "#333",
                placeholder: 'Farbe'
            },
            {
                name: "Text Selection",
                id: "txt",
                // component: textInput,
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
    // develop

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
