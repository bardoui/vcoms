<template>
    <div style="display: inline-block">
        {{
            val ? (format == "ago" ? val!.fromNow() : val!.format(format)) : alter
        }}
    </div>
</template>

<script lang="ts" setup>
import { defineProps, computed } from "@vue/runtime-core";
import { parse } from "@bardoui/date-utils";

const props = defineProps({
    format: {
        type: String,
        default: "jDD jMMMM jYYYY ساعت HH:mm:ss"
    },
    alter: {
        type: String,
        default: "-"
    },
    value: { required: true }
});

const val = computed(() => {
    const v = parse((props.value as any) || "");
    return v && v != null && v.isValid() ? v.locale("fa") : null;
});
</script>
