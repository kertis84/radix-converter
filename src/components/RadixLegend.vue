<template>
    <div class="mt-3">
        <button class="btn btn-secondary my-3" @click="legend_shown = !legend_shown">
            Посмотреть легенду (символ - значение)
        </button>
    </div>
    <div class="collapse mb-3" :class="[legend_shown && 'show']">
        <div class="row ms-1">
            <div class="card w-75px text-center me-1 mb-1" v-for="(char, idx) in radix64_chars" :key="[char,'-', idx].join('')">{{ [char,' - ', idx].join('') }}</div>
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

const legend_shown = ref(false)

</script>

<style scoped>
.w-75px {
    width: 75px;
}
</style>