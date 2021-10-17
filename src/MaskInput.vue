<template>
    <input class="asdf" v-mask="maskOptions" v-model="value" />
</template>

<script lang="ts" setup>
import { defineProps, defineEmits, onMounted, watch } from "vue";
import { vMask } from "@bardoui/vmask";
import { computed, ref } from "@vue/reactivity";

// Props and emmits
const emits = defineEmits(["update:modelValue"]);
const props = defineProps({
    modelValue: String,
    options: { required: false },
    mask: { required: true },
    guide: { type: String, default: "" }
});

// Core
const _v = ref("");
const value = computed({
    get: () => _v.value,
    set: v => {
        _v.value = v;
        emits("update:modelValue", v);
    }
});
const maskOptions = computed(() => ({
    mask: props.mask,
    options: Object.assign({}, props.options, {
        guide: !!props.guide,
        placeholderChar: props.guide || "_"
    })
}));
onMounted(() =>
    watch(
        () => props.modelValue,
        v => (value.value = v || ""),
        { immediate: true }
    )
);
</script>
