<template>
    <div>
        <div class="max-w-[450px] m-auto mt-8 border border-gray-300 bg-white px-8 py-5 rounded-xl">
            <div class="flex flex-col items-center justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-24 h-24 fill-blue-400">
                    <path fill-rule="evenodd"
                        d="M12 1.5a5.25 5.25 0 00-5.25 5.25v3a3 3 0 00-3 3v6.75a3 3 0 003 3h10.5a3 3 0 003-3v-6.75a3 3 0 00-3-3v-3c0-2.9-2.35-5.25-5.25-5.25zm3.75 8.25v-3a3.75 3.75 0 10-7.5 0v3h7.5z"
                        clip-rule="evenodd" />
                </svg>
                <h1 class="text-[28px] font-bold mt-4">
                    PASSWORD GENERATOR
                </h1>
                <p class="text-center my-2">
                    Create strong and secure passwords to keep your account safe online.
                </p>
            </div>
            <div class="flex items-center justify-between gap-4 mt-2">
                <div class="w-full relative">
                    <input class="w-full border rounded-lg px-2 py-1" type="text" v-on:focus="$event.target.select()"
                        ref="clone" v-model="generatedPassword">
                    <span class="absolute right-0 mt-[6px] mr-2 cursor-pointer" @click="refreshPass = !refreshPass">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"
                            class="w-5 h-5 fill-gray-400 hover:fill-gray-600 transition-all">
                            <path fill-rule="evenodd"
                                d="M15.312 11.424a5.5 5.5 0 01-9.201 2.466l-.312-.311h2.433a.75.75 0 000-1.5H3.989a.75.75 0 00-.75.75v4.242a.75.75 0 001.5 0v-2.43l.31.31a7 7 0 0011.712-3.138.75.75 0 00-1.449-.39zm1.23-3.723a.75.75 0 00.219-.53V2.929a.75.75 0 00-1.5 0V5.36l-.31-.31A7 7 0 003.239 8.188a.75.75 0 101.448.389A5.5 5.5 0 0113.89 6.11l.311.31h-2.432a.75.75 0 000 1.5h4.243a.75.75 0 00.53-.219z"
                                clip-rule="evenodd" />
                        </svg>
                    </span>
                </div>
                <div @click="copy"
                    class="h-[34px] bg-blue-400 px-2 py-1 rounded-lg flex items-center gap-1 text-white cursor-pointer hover:bg-blue-500 transition-all">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-5 h-5">
                        <path
                            d="M7 3.5A1.5 1.5 0 018.5 2h3.879a1.5 1.5 0 011.06.44l3.122 3.12A1.5 1.5 0 0117 6.622V12.5a1.5 1.5 0 01-1.5 1.5h-1v-3.379a3 3 0 00-.879-2.121L10.5 5.379A3 3 0 008.379 4.5H7v-1z" />
                        <path
                            d="M4.5 6A1.5 1.5 0 003 7.5v9A1.5 1.5 0 004.5 18h7a1.5 1.5 0 001.5-1.5v-5.879a1.5 1.5 0 00-.44-1.06L9.44 6.439A1.5 1.5 0 008.378 6H4.5z" />
                    </svg>

                    <p class="text-[16px] select-none">
                        COPY
                    </p>
                </div>
            </div>
            <div class="flex items-center gap-2 mt-6 select-none">
                <div>
                    Password Count:
                </div>
                <input class="border rounded-lg px-2 py-1 w-14" v-model="passLength" type="text">
            </div>
            <div class="mt-6">
                <div class="slide_container flex items-center">
                    <input v-model="passLength" min="8" max="16" class="w-full" type="range">
                </div>
                <div class="options">
                    <div class="flex justify-between mt-2 items-center gap-2" v-for="(option, index) in optiondata"
                        :key="index">
                        <div>{{ option.name }}</div>
                        <input class="h-4 w-4" type="checkbox" v-model="option.status">
                    </div>
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
                    name: 'LowerCase',
                    status: true,
                    chars: 'abcdefghjkmnopqrstuvwxyz'
                },
                {
                    name: 'Uppercase',
                    status: true,
                    chars: 'ABCDEFGHJKLMNOPQRSTUVWXYZ'
                },
                {
                    name: 'Numbers',
                    status: true,
                    chars: '0123456789'
                },
                {
                    name: 'Special',
                    status: true,
                    chars: '!$%&?+*#-/'
                }
            ]
        }
    },
    methods: {
        copy() {
            this.$refs.clone.focus();
            document.execCommand('copy');
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