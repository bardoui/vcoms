<template>
    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3 class="is-marginless">Format Data</h3>
                    <div class="meta">Numbers & Dates</div>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section">
                <div class="gaper is-auto">
                    <vNumber :value="123456" />
                    <vNumber :value="-3123123" />
                    <vJalaali value="2022-01-01T12:00:00Z" class="is-rtl" />
                    <vJalaali
                        value="2022-01-01T12:00:00Z"
                        format="ago"
                        class="is-rtl"
                    />
                </div>
            </div>
        </div>
    </section>
</template>

<script lang="ts" setup>
import { vJalaali, vNumber } from "@/vComs";
</script>
