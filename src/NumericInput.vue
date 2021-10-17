<template>
    <input
        :inputmode="decimal ? 'decimal' : 'numeric'"
        v-mask="maskOptions"
        v-model="value"
    />
</template>

<script lang="ts" setup>
import { ref, defineProps, defineEmits, watch, onMounted } from "vue";
import { mask, vMask } from "@bardoui/vmask";
import { computed } from "@vue/reactivity";
import { extractNumeric, formatNumber, parseNumber } from "@bardoui/utils";

// Props and emmits
const emits = defineEmits(["update:modelValue", "format"]);
const props = defineProps({
    prefix: { type: String, default: "" },
    suffix: { type: String, default: "" },
    separator: { type: String, default: "," },
    decimal: { type: Number, default: 0 },
    min: { type: Number, default: 0 },
    max: { type: Number, default: Number.MAX_SAFE_INTEGER },
    modelValue: Number
});

// Core
const maskOptions = computed(() => {
    const { mask, pipe } = useNumeral(
        props.separator || "",
        props.decimal || 0,
        props.prefix || "",
        props.suffix || "",
        props.min || 0,
        props.max || 0
    );
    return { mask, options: { pipe } };
});
const _v = ref("");
const value = computed({
    get: () => _v.value,
    set: v => {
        _v.value = v;
        emits("format", v);
        emits("update:modelValue", +extractNumeric(v) || 0);
    }
});
// Watch props change
onMounted(() => {
    watch(
        () => props.modelValue,
        v => {
            if ((parseNumber(_v.value) || 0) !== (v || 0)) {
                value.value = (v || 0).toString();
            }
        },
        { immediate: true }
    );
});

// Numeral formatter helper
function useNumeral(
    separator: string,
    decimal: number,
    prefix: string,
    suffix: string,
    min: number,
    max: number
) {
    const integerPart = (s: string): string => s.split(".")[0] || "";
    const fractionPart = (s: string): string => s.split(".")[1] || "";
    const isSpecialState = (s: string) =>
        ["", "-", "-0.", "0.", "0", "-0"].includes(s);
    // Formatter
    const _mask = (raw: string) =>
        prefix
            .concat(formatNumber(extractNumeric(raw), separator))
            .concat(suffix)
            .split("")
            .map(ch => (/\d/.test(ch) ? /\d/ : { "-": /[-]/ }[ch] || ch));
    const _pipe = (conformed: string) => {
        // Parse
        let sgn = extractNumeric(conformed)[0] === "-" ? "-" : ""; // sign
        const raw = extractNumeric(conformed).substring(sgn ? 1 : 0); // unsigned value
        const sep = decimal > 0 && raw.includes(".") ? "." : ""; // decimal separator
        min >= 0 && (sgn = "");
        // Integer
        let integer = integerPart(raw);
        if (integer.length > 1 && integer.startsWith("0")) {
            integer = integer.substring(1);
        } // remove leading zero
        // Fraction
        let fraction = fractionPart(raw).substring(0, decimal);
        // Generate result number or return special state
        const num = `${sgn}${integer}${sep}${fraction}`;
        if (!isSpecialState(num) && !isNaN(+num)) {
            if (+num > max) return mask(max.toString(), _mask);
            if (+num < min) return mask(min.toString(), _mask);
        }
        return mask(num, _mask);
    };

    return { mask: _mask, pipe: _pipe };
}
</script>
