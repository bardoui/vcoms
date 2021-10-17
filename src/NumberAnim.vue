<template>
    <slot :formatted="formatted" :value="value">{{ formatted }}</slot>
</template>

<script lang="ts" setup>
import { computed, defineProps, onMounted, ref, watch } from "vue";
import { formatNumber } from "@bardoui/utils";
import anime, { AnimeInstance } from "animejs";

const props = defineProps({
    value: { type: Number, required: true },
    decimals: { type: Number, default: 0 },
    separator: { type: String, default: "," },
    easing: { type: String, default: "cubicBezier(0.25, 0.46, 0.45, 0.94)" },
    duration: { type: Number, default: 750 }
});

const val = ref(0);
const value = computed(() => +val.value.toFixed(props.decimals));
const formatted = computed(() => formatNumber(value.value, props.separator));
let anim: AnimeInstance | null = null;
onMounted(() =>
    watch(
        () => props.value,
        v => {
            if (anim != null) {
                anim.pause();
                anim = null;
            }
            anim = anime({
                targets: val,
                value: v,
                easing: props.easing,
                duration: props.duration,
                complete() {
                    val.value = v;
                }
            });
        },
        { immediate: true }
    )
);
</script>
