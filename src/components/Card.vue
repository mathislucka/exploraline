<template>
    <div
        class="flex flex-col rounded-sm border-gray-100 max-w-6xl ml-auto mr-auto p-4 text-left leading-8 tracking-normal">
        <div class="border-t border-gray-200">
            <div class="w-4 inline-block mr-2 mt-3 rounded-sm" :style="{ 'background-color': colorCode }">&nbsp;</div>
            <span class="text-lg pt-3">{{ content }}</span>
        </div>
    </div> 
</template>

<script>
import ColorHash from 'color-hash'

const colorHasher = new ColorHash()

export default {
    'name': 'Card',
    props: {
        content: {
            type: String,
            required: true
        }
    },
    computed: {
        colorCode () {
            return colorHasher.hex(this.content)
        }
    }
}
</script>
