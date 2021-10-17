<template>
    <section>
        <h3>Masked Input</h3>
        <v-mask-input mask="###-###" v-model="v_mask" />
    </section>
    <section>
        <h3>Numeric Input</h3>
        <v-numeric-input
            :decimal="3"
            :min="-1000"
            :max="Number.MAX_SAFE_INTEGER"
            prefix="$ "
            suffix=" #"
            separator=" "
            v-model="v_num"
            @format="v_formatted = $event"
        />
        <br />
        <button @click="v_num = 1010">Change 1010</button>
        <button @click="v_num = -1000">Change -1000</button>
        <button @click="v_num = 0.23321">Change 0.23321</button>
        <button @click="v_num = 0">Change 0</button>
        <br />
        <br />
        <p>
            <strong>Raw:</strong>
            <v-number-anim
                :value="v_num"
                :decimals="3"
                separator="/"
                easing="linear"
                :duration="500"
            >
                <template #="{ formatted, value }">
                    <h1>{{ formatted }}</h1>
                    <p>{{ value }}</p>
                </template>
            </v-number-anim>
            <span>{{ v_num }}</span>
            <br />
            <strong>Formatted:</strong>
            <span>{{ v_formatted }}</span>
        </p>
    </section>
    <section>
        <h3>Jalali Input</h3>
        <blockquote>Minimum is 1400-01-03</blockquote>
        <strong>{{ v_date }}</strong>
        <v-jalali-input separator="/" min="1400-01-3" v-model="v_date" />
        <button @click="v_date = '1400-12-33'">1400-12-33</button>
        <button @click="v_date = '1400-1-1'">1400-1-1</button>
    </section>
</template>

<script lang="ts" setup>
import { vMaskInput, vNumericInput, vJalaliInput, vNumberAnim } from "@/vComs";
import { ref } from "vue";
const v_mask = ref("1234");
const v_num = ref(1234);
const v_formatted = ref("");
const v_date = ref("1400-3-2");
</script>
