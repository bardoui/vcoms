<template>
    <div class="layout">
        <div class="attachment">
            <div class="container is-padding-less">
                <h2 class="is-shade-colored is-margin-less">
                    Components For Vue 3
                </h2>
            </div>
        </div>
        <div class="content">
            <div class="wrapper">
                <div class="container is-padding-less">
                    <inputs />
                    <section>
                        <input inputmode="decimal" />
                        <input inputmode="numeric" />
                        <input inputmode="email" />
                        <input inputmode="url" />
                    </section>
                </div>
            </div>
        </div>
    </div>
</template>
<script lang="ts" setup>
import Inputs from "./Inputs.vue";
</script>

<style lang="scss">
@import "./app.scss";
</style>
