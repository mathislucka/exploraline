<template>
    <header class="flex items-stretch text-lg text-blue-500 font-semibold">
        <div class="p-4 pt-6">
            <label class="bg-blue-600 hover:bg-blue-900 text-white text-md font-normal shadow-md pointer rounded-sm px-4 py-2 cursor-pointer">
                Upload jsonl
                <input type="file" class="hidden" @change="readJsonl" accept=".jsonl">
            </label>
        </div>
        <div class="p-4 pt-6" v-if="fileName !== ''">
            <span class="mr-4">{{ fileName }}</span>
            <label>
                [
            </label>
            <input class="w-8 leading-6 mr-2 text-center bg-neutral-50" type="text" v-model="itemStart">
            <span class="mr-2">:</span>
            <input type="text" class="w-8 leading-8 text-center bg-neutral-50" v-model="itemEnd">
            <label class="">
                ]
            </label>
            <span class="ml-2">({{ jsonList.length }})</span>
        </div>
        <div class="grow"></div>
        <div class="p-4 pt-6 border-b border-l border-black">
            <label for="search" class="mr-2">Search</label>
            <input
                :value="searchTerm"
                @input="triggerSearch"
                type="text"
                id="search"
                class="rounded-md border border-blue-500 bg-neutral-50 text-black leading-8 pl-1 w-64">
        </div>
        <h1 class="text-2xlfont-bold text-blue-900 border-l border-b border-black p-4 pt-6">explorAline</h1>
    </header>
    <nav class="flex p-4 font-semibold text-lg text-blue-500" v-if="jsonList.length > 0">
        <ul class="flex w-full flex-wrap">
            <li class="mr-4">
                Display
            </li>
            <li
                v-for="(key, idx) in Object.keys(jsonList[0])" :key="idx"
                class="mr-4">
                <label class="font-semibold text-lg">
                    <input type="checkbox" @change="toggleDisplayProp(key)" :checked="displayProps.includes(key)">
                    {{ key }}
                </label>
            </li>
        </ul>
    </nav>
    <div class="flex">
        <div v-for="prop in displayProps" :key="prop" class="basis-0 grow ml-auto mr-auto text-gray-600 font-semibold px-4 mb-1 text-lg max-w-6xl">{{ prop }}</div>
    </div>
    <main class="grow overflow-y-auto px-2" v-if="displayProps.length > 0">
        <line-display
            class="mb-4"
            v-for="(item, idx) in displayItems"
            :key="idx"
            :displayItem="item"
            :displayProps="displayProps" />
    </main>
</template>

<script>
import LineDisplay from './LineDisplay.vue'

export default {
    'name': 'explorAline',
    components: {
        LineDisplay
    },
    data () {
        return {
            itemStart: 0,
            itemEnd: 10,
            searchTerm: '',
            fileName: '',
            displayProps: [],
            jsonList: []
        }
    },
    computed: {
        displayItems () {
            let displayList = this.jsonList
            if (this.searchTerm !== '') {
                displayList = displayList.filter(item => {
                    const joinedProps = Object.entries(item).reduce((acc, [key, value]) => {
                        if (this.displayProps.includes(key)) {
                            acc += value
                        }
                        return acc
                    }, '')

                    return joinedProps.match(RegExp(this.searchTerm, 'ig'))
                })
            }

            return displayList.slice(this.itemStart, this.itemEnd)
        }
    },
    methods: {
        readJsonl(event) {
            const files = event.target.files

            if (files.length > 0) {
                const file = files[0]
                this.fileName = file.name
                file.text().then(text => {
                    const splittedText = text.split('\n').filter(line => line !== '')
                    console.log(splittedText)
                    this.jsonList = splittedText.map(line => JSON.parse(line))
                })
            }

        },

        toggleDisplayProp(prop) {
            const displayProps = this.displayProps.filter(item => item !== prop)

            if (displayProps.length === this.displayProps.length) {
                displayProps.push(prop)
            }

            this.displayProps = displayProps
        },

        triggerSearch(e) {
            const value = e.target.value
            this.itemStart = 0
            this.itemEnd = 10
            if (value.length > 3) {
                this.searchTerm = value
            } else {
                this.searchTerm = ''
            }
        }
    }
}
</script>
