<template>
    <slot name="error" :error="error" v-if="isFailed">{{ error }}</slot>
    <slot name="empty" v-else-if="isEmpty">No Records</slot>
    <slot :records="records" v-else></slot>
</template>

<script lang="ts" setup>
import { defineProps, computed } from "vue";
const props = defineProps({ data: Array, error: { default: null } });
const records = computed(() => (Array.isArray(props.data) ? props.data : []));
const isEmpty = computed(() => records.value.length == 0);
const isFailed = computed(() => !!props.error);
</script>
