<template>
    <div class="mt-3">
        <button class="btn w-50px" v-for="i in 63" :key="'btn-' + (i + 1)"
            :class="[radix_array.includes(i + 1) ? 'btn-light' : 'btn-outline-light']" @click="emit('toggle-radix', i + 1)">
            {{ i + 1 }}
        </button>
    </div>
</template>

<script setup lang="ts">
interface Props {
    radix_array: Array<number>
}

defineProps<Props>()

const emit = defineEmits<{ (e: 'toggle-radix', radix: number): void }>()

</script>

<style scoped>
.w-50px {
    width: 50px;
}
</style>