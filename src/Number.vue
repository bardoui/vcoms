<template>
    <div style="display: inline-block; direction: ltr">
        {{ formatNumber(value, separator || ",") }}
    </div>
</template>

<script lang="ts" setup>
import { defineProps } from "vue";
import { formatNumber } from "@bardoui/utils";
defineProps({ value: Number, separator: String });
</script>
