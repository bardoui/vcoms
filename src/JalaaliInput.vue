<template>
    <input
        :inputmode="'numeric'"
        v-mask="maskOptions"
        v-model="value"
        @blur="norm"
    />
</template>

<script lang="ts" setup>
import {
    ref,
    defineProps,
    defineEmits,
    watch,
    onMounted,
    watchEffect
} from "vue";
import { vMask } from "@bardoui/vmask";
import { computed } from "@vue/reactivity";
import { parse, parseFrom } from "@bardoui/date-utils";

// Props and emmits
const emits = defineEmits(["update:modelValue"]);
const props = defineProps({
    separator: { type: String, default: "-" },
    min: { type: String, default: "" },
    max: { type: String, default: "" },
    modelValue: String
});

// Helpers
const unifySeparator = (v: string, r: string) => v.replace(/[^0-9]+/g, r);
const normalizeNum = (num: string, min: number, max: number) => {
    if (num && !isNaN(+num)) {
        let n = +num;
        if (n < min) {
            n = min;
        }
        if (n > max) {
            n = max;
        }
        return `${n}`.padStart(2, "0");
    }
    return num;
};
const daysInMonth = (y: string, m: string) => {
    const dt = parseFrom(`${y}-${m}-01`, "jYYYY-jMM-jDD");
    return dt.isValid() ? dt.endOf("jMonth").jDate() : 31;
};
const explode = (d: string) => {
    let parts = [];
    const date = unifySeparator(d, "-");
    if (date.includes("-")) {
        parts = date.split("-");
    } else {
        parts = [d.substring(0, 4), d.substring(4, 6), d.substring(6, 8)];
    }
    return {
        year: parts[0] || "",
        month: parts[1] || "",
        day: parts[2] || ""
    };
};
const normalize = (date: string) => {
    // normalize
    let { year, month, day } = explode(date);
    month = normalizeNum(month, 1, 12);
    day = normalizeNum(day, 1, daysInMonth(year, month));
    let _date = [year, month, day].join(props.separator);
    // Min/Max
    const now = parseFrom(unifySeparator(_date, "-"), "jYYYY-jMM-jDD");
    const min = parseFrom(unifySeparator(props.min, "-"), "jYYYY-jMM-jDD");
    const max = parseFrom(unifySeparator(props.max, "-"), "jYYYY-jMM-jDD");
    if (now.isValid() && min.isValid() && now.isBefore(min)) {
        _date = min.format(["jYYYY", "jMM", "jDD"].join(props.separator));
    }
    if (now.isValid() && max.isValid() && now.isAfter(max)) {
        _date = max.format(["jYYYY", "jMM", "jDD"].join(props.separator));
    }
    return _date;
};

// Core
const pipe = (separator: string) => (conformed: string) => {
    let { year, month, day } = explode(conformed);

    if (month && +month > 1) {
        month = normalizeNum(month, 1, 12);
    }

    if (day && +day > 3) {
        day = normalizeNum(day, 1, daysInMonth(year, month));
    }

    return [year, month, day].filter(Boolean).join(separator);
};
const maskOptions = computed(() => ({
    mask: ["####", "##", "##"].join(props.separator),
    options: { pipe: pipe(props.separator) }
}));

const _v = ref("");
const value = computed({
    get: () => _v.value,
    set: v => (_v.value = v)
});
const norm = () => (value.value = normalize(value.value));

// Watch props change
onMounted(() => {
    watch(
        () => props.modelValue,
        v => {
            const dt = parse(v || "-");
            if (dt.isValid()) {
                const jd = dt.format(
                    ["jYYYY", "jMM", "jDD"].join(props.separator)
                );
                if (jd != value.value) {
                    value.value = normalize(jd);
                }
            }
        },
        { immediate: true }
    );

    watchEffect(() => {
        const raw = unifySeparator(_v.value, "");
        const dt = parseFrom(raw.length == 8 ? raw : "", "jYYYYjMMjDD");
        if (dt.isValid()) {
            emits("update:modelValue", dt.format());
        } else {
            emits("update:modelValue", "");
        }
    });
});
</script>
