<template>
    <div class="flex w-full">
        <card class="basis-0 grow" v-for="prop in displayProps" :key="prop" :content="displayItem[prop]" />
    </div> 
</template>

<script>
import Card from './Card.vue'
export default {
    'name': 'LineDisplay',
    components: {
        Card
    },
    props: {
        displayProps: {
            type: Array,
            required: true
        },
        displayItem: {
            type: Object,
            required: true
        }
    },
}
</script>
