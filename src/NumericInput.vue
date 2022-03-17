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
import { extractNumeric, parseNumber } from "@bardoui/utils";

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
    get: () => _v.value || "0",
    set: v => {
        _v.value = v;
        emits("format", v);
        emits("update:modelValue", parseNumber(v) || 0);
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
    // helpers
    const explodeNum = (num: string, normalize: boolean) => {
        num = extractNumeric(num);
        // get sign
        const sign = num.startsWith("-") ? "-" : "";
        num = num.replace(/[-]/g, "");

        // get decimal separator
        const sep = num.includes(".") ? "." : "";

        // normalize
        if (normalize) {
            num = num.replace(/^0+/, "0");
            if (num.match(/^[0][1-9]/)) {
                num = num.replace(/^0+/, "");
            }
        }

        // get parts
        const integer = num.split(".")[0] || "";
        const fraction = num.split(".")[1] || "";

        return {
            sign,
            sep,
            integer,
            fraction
        };
    };
    const isSpecialState = (s: string) => ["", "-", "-0", "0"].includes(s); // || s.endsWith(".");

    // Formatter
    const _mask = (raw: string) => {
        let { sign, sep, integer, fraction } = explodeNum(raw, false);
        const chars = integer.split("").reverse();

        const out: string[] = [];
        let counter = 1;
        for (const ch of chars) {
            out.push(ch);
            if (counter == 3) {
                out.push(separator);
                counter = 1;
            } else {
                counter++;
            }
        }

        if (out[out.length - 1] == separator) {
            out.pop();
        }

        raw = sign
            .concat(out.reverse().join(""))
            .concat(sep)
            .concat(fraction);
        return prefix
            .concat(raw)
            .concat(suffix)
            .split("")
            .map(ch => (/\d/.test(ch) ? /\d/ : { "-": /[-]/ }[ch] || ch));
    };
    const _pipe = (conformed: string) => {
        // Parse
        let { sign, sep, integer, fraction } = explodeNum(conformed, true);
        if (min >= 0) {
            sign = "";
        }
        if (decimal <= 0) {
            sep = "";
        }
        fraction = fraction.substring(0, Math.max(0, decimal));
        const num = `${sign}${integer}${sep}${fraction}`;
        if (!isSpecialState(num) && !isNaN(+num)) {
            if (+num > max) return mask(max.toString(), _mask);
            if (+num < min) return mask(min.toString(), _mask);
        }
        return mask(num, _mask);
    };

    return { mask: _mask, pipe: _pipe };
}
</script>
