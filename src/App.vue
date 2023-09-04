<template>
    <div class="container pt-5">
        <div class="mt-5 fs-1 text-light">Онлайн калькулятор систем счисления</div>
        <div class="mt-5 fs-3 text-light">Выберите системы счисления</div>
        <div class="row">
            <div class="col-md-6">
                <RadixSelect :radix_array="radix_array" v-on:toggle-radix="toggleRadix" :radix64_chars="radix64_chars" />
                <RadixLegend :radix64_chars="radix64_chars" />
            </div>
            <div class="col-md-6 scrollable">
                <RadixInputs :radix_array="radix_array" :radix64_chars="radix64_chars" />
            </div>
        </div>
    </div>
    <div class="copyright"><a href="https://github.com/kertis84/radix-converter/blob/main/LICENSE" title="MIT License">Copyright &copy; 2023 kertis84</a></div>
</template>

<script setup lang="ts">
import RadixSelect from './components/RadixSelect.vue'
import RadixLegend from './components/RadixLegend.vue'
import RadixInputs from './components/RadixInputs.vue'
import { ref } from 'vue';

const radix_array = ref([2, 4, 8, 10, 12, 16, 32, 48, 64])
const radix64_chars = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz[]"

const toggleRadix = (radix: number) => {
    const idx = radix_array.value.indexOf(radix)
    idx >= 0 ? radix_array.value.splice(idx, 1) : radix_array.value.push(radix)
}

</script>

<style scoped>
.copyright {
    position: fixed;
    right: 0;
    bottom: 0;
    background-color: white;
}
@media (min-width: 768px) {
    .scrollable {
        max-height: 65vh;
        overflow-y: auto;
    }
}
</style>
