<template>
    <form>
        <div class="mt-3 row" v-for="radix in radix_array" :key="'radix-' + radix">
            <label class="col-1 col-form-label text-light text-end" :for="'radix-' + radix">{{ radix }}</label>
            <div class="col-11">
                <input class="form-control" type="text" :name="radix.toString()" :id="'radix-' + radix"
                    :value="getRadixBasedValue(radix)" @input="onInputValue" :maxlength="Math.ceil(53 / Math.log2(radix))" />
                <small v-if="radix === input_error_index" class="form-text text-warning">* для данного основания допустимы
                    символы: {{ getAllowedCharsByRadix(radix) }}</small>
            </div>
        </div>
    </form>
</template>

<script setup lang="ts">
import { ref, toRefs } from 'vue'

interface Props {
    radix_array: Array<number>
    radix64_chars: string
}

const props = defineProps<Props>()
const { radix64_chars } = toRefs(props)
const num = ref(0)
const input_error_index = ref(-1)

const getAllowedCharsByRadix = (radix: number) => {
    let chars = ''
    radix > 63 && (chars = ']' + chars)
    radix > 62 && (chars = ', [' + chars)
    radix > 37 && (chars = '-' + radix64_chars.value[radix < 62 ? radix - 1 : 61] + chars)
    radix >= 37 && (chars = ', a' + chars)
    radix > 11 && (chars = '-' + radix64_chars.value[radix < 36 ? radix - 1 : 35] + chars)
    radix >= 11 && (chars = ', A' + chars)
    radix > 0 && (chars = '0-' + radix64_chars.value[radix < 10 ? radix - 1 : 9] + chars)
    return chars
}

const getRadixBasedValue = (radix: number) => {
    let res = ''
    let x = num.value
    while (x >= radix) {
        res = radix64_chars.value[x % radix] + res
        x = Math.floor(x / radix)
    }
    res = radix64_chars.value[x] + res
    return res
}

const validateInput = (input: string, radix: number) => {
    const subset = radix64_chars.value.substring(0, radix)
    for (let i = 0; i < input.length; i++) {
        if (!subset.includes(input[i]))
            return false
    }
    return true
}

const onInputValue = (e: Event) => {
    const target = e.target as HTMLInputElement
    let res = 0
    const radix = Number(target.name)
    if (validateInput(target.value, radix)) {
        for (let i = 0; i < target.value.length; i++) {
            const val = radix64_chars.value.indexOf(target.value[i])
            res += val * Math.pow(radix, target.value.length - i - 1)
        }
        input_error_index.value = -1
        num.value = res
    }
    else {
        input_error_index.value = Number(target.name)
        target.value = getRadixBasedValue(input_error_index.value)
    }
}

</script>

<style type="scope"></style>