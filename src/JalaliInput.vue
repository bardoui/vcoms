<template>
    <input
        :inputmode="'numeric'"
        v-mask="maskOptions"
        v-model="value"
        @blur="norm"
    />
</template>

<script lang="ts" setup>
import { ref, defineProps, defineEmits, watch, onMounted } from "vue";
import { vMask } from "@bardoui/vmask";
import { computed } from "@vue/reactivity";
import moment from "jalali-moment";

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
const daysInMonth = (y: string, m: string) => {
    const d = moment.from(`${y}-${m}-01`, "fa");
    return +y && +m && d.isValid() ? d.jDaysInMonth() : 31;
};
const explode = (d: string) => {
    let parts = [];
    const date = unifySeparator(d, "-");
    if (date.includes("-")) {
        parts = date.split("-");
    } else {
        parts = [d.substring(0, 4), d.substring(4, 6), d.substring(6, 8)];
    }
    return [
        unifySeparator(parts[0] || "", ""),
        unifySeparator(parts[1] || "", ""),
        unifySeparator(parts[2] || "", "")
    ];
};
const normalize = (date: string) => {
    let [y, m, d] = explode(date);
    m.length == 1 && (m = `0${m}`);
    d.length == 1 && (d = `0${d}`);
    let _date = [y, m, d].join(props.separator);
    // Min/Max
    const mDate = moment.from(unifySeparator(_date, "-"), "fa", "YYYY-MM-DD");
    if (mDate.isValid()) {
        const min = moment.from(
            unifySeparator(props.min, "-"),
            "fa",
            "YYYY-MM-DD"
        );
        if (min.isValid() && mDate.isBefore(min)) {
            _date = min.format(["jYYYY", "jMM", "jDD"].join(props.separator));
        }
        const max = moment.from(
            unifySeparator(props.max, "-"),
            "fa",
            "YYYY-MM-DD"
        );
        if (max.isValid() && mDate.isAfter(max)) {
            _date = max.format(["jYYYY", "jMM", "jDD"].join(props.separator));
        }
    }
    return _date;
};
const pipe = (separator: string) => (conformed: string) => {
    let [year, month, day] = explode(conformed);

    if (month) {
        month.length < 2 && (+month > 1 || +day) && (month = "0" + month);
        +month > 12 && (month = "12");
        month == "00" && (month = "01");
    }

    if (day) {
        day.length < 2 && +day > 3 && (day = "0" + day);
        day == "00" && (day = "01");
        +day > daysInMonth(year, month) &&
            (day = `${daysInMonth(year, month)}`);
    }

    return [year, month, day].filter(Boolean).join(separator);
};

// Core
const maskOptions = computed(() => ({
    mask: ["####", "##", "##"].join(props.separator),
    options: { pipe: pipe(props.separator) }
}));

const _v = ref("");
const value = computed({
    get: () => _v.value,
    set: v => {
        _v.value = v;
        emits("update:modelValue", v);
    }
});
const norm = () => (value.value = normalize(value.value));

// Watch props change
onMounted(() => {
    watch(
        () => props.modelValue,
        v => v != value.value && (value.value = normalize(v || "")),
        { immediate: true }
    );
});
</script>
