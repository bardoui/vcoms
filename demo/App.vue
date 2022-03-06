<template>
    <div class="layout">
        <div class="attachment">
            <div class="container is-paddingless">
                <h2 class="is-margin-less">
                    Components For Vue 3
                </h2>
            </div>
        </div>
        <div class="content">
            <div class="wrapper">
                <div class="container is-paddingless">
                    <inputs />
                    <data-view />
                </div>
            </div>
        </div>
    </div>
</template>
<script lang="ts" setup>
import Inputs from "./Inputs.vue";
import DataView from "./DataView.vue";
</script>

<style lang="scss">
@import "./app.scss";
</style>
