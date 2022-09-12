<template>
    <div>
        <div class="max-w-[400px] m-auto mt-8 border border-gray-300 bg-white px-4 py-2 rounded">
            <h1 class="text-[18px]">
                Выбрать настройки:
            </h1>
            <div class="mt-6">
                <div class="slide_container flex items-center">
                    <label class="text-[18px] mr-4" for="passRange">Длина: {{ passLength }}</label>
                    <input v-model="passLength" min="8" max="16" class="w-[calc(100%-10rem)]" type="range">
                </div>
                <div class="options">
                    <div class="flex items-center gap-2" v-for="(option, index) in optiondata" :key="index">
                        <input type="checkbox" v-model="option.status">
                        <div>{{ option.name }}</div>
                    </div>
                </div>

                <input class="mt-1 border rounded px-1" type="text" name="" id="" v-model="generatedPassword">

                <div class="mt-2">
                    <button @click="refreshPass = !refreshPass"
                        class="border border-gray-300 rounded text-gray-400 px-2 py-1 rounded transition-all hover:border-gray-600 hover:text-gray-800">Сбросить</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            passLength: 12,
            refreshPass: false,
            optiondata: [
                {
                    name: 'Нижний регистр',
                    status: true,
                    chars: 'abcdefghjkmnopqrstuvwxyz'
                },
                {
                    name: 'Верхний регистр',
                    status: true,
                    chars: 'ABCDEFGHJKLMNOPQRSTUVWXYZ'
                },
                {
                    name: 'Числа',
                    status: true,
                    chars: '0123456789'
                },
                {
                    name: 'Символы',
                    status: true,
                    chars: '!$%&?+*#-/'
                }
            ]
        }
    },
    computed: {
        charset() {
            return [...this.optiondata]
                .map(element => {
                    if (element.status === true)
                        return element.chars;
                }).join('');
        },
        generatedPassword() {
            this.refreshPass;
            return [...window.crypto.getRandomValues(new Uint32Array(this.passLength))]
                .map(value => this.charset[value % this.charset.length])
                .join('');
        }
    }
}
</script>

<style>

</style>