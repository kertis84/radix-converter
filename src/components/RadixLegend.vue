<template>
    <div class="mt-3">
        <button class="btn btn-secondary my-3" @click="legendShown = !legendShown">
            Посмотреть легенду (символ - значение)
        </button>
    </div>
    <div class="collapse mb-3" id="collapseExample" :class="[legendShown && 'show']">
        <div class="row ms-1">
            <div class="card w-75px center text-center me-1 mb-1" v-for="(char, idx) in radix64_chars">{{ [char,' - ', idx].join('') }}</div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, toRefs } from 'vue'

interface Props {
    radix64_chars: string
}

const props = defineProps<Props>()
const { radix64_chars } = toRefs(props)

const legendShown = ref(false)

</script>

<style scoped>
.w-75px {
    width: 75px;
}
</style>