<template>
    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3>Masked Input</h3>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section">
                <div class="meta">Fill ###-### mask</div>
                <div class="input">
                    <v-mask-input mask="###-###" v-model="v_mask" />
                </div>
            </div>
        </div>
    </section>

    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3>Numeric Input</h3>
                    <div>
                        <div class="gaper is-auto">
                            <div class="link is-shade" @click="v_num = 1234567">
                                [ 1,234,567 ]
                            </div>
                            <div class="link is-shade" @click="v_num = -10000">
                                [ -10,000 ]
                            </div>
                            <div class="link is-shade" @click="v_num = 0.23321">
                                [ 0.23321 ]
                            </div>
                            <div class="filler"></div>
                            <div class="divder"></div>
                            <div class="link is-error" @click="v_num = 0">
                                Reset To 0
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section">
                <div class="input">
                    <v-numeric-input
                        :decimal="3"
                        :min="-1000"
                        :max="Number.MAX_SAFE_INTEGER"
                        prefix="$ "
                        suffix=" #"
                        separator=","
                        v-model="v_num"
                        @format="v_formatted = $event"
                    />
                </div>
                <br />

                <div>
                    <div class="gaper is-auto">
                        <div class="meta">Separator</div>
                        <strong class="meta is-primary">,</strong>
                        <div class="divider"></div>
                        <div class="meta">Min</div>
                        <strong class="meta is-primary">-1,000</strong>
                        <div class="divider"></div>
                        <div class="meta">Decimals</div>
                        <strong class="meta is-primary">3 Points</strong>
                        <div class="filler"></div>
                        <div class="meta">Raw</div>
                        <strong class="meta is-primary">{{ v_num }}</strong>
                        <div class="divider"></div>
                        <div class="meta">Formatted</div>
                        <strong class="meta is-primary">{{
                            v_formatted
                        }}</strong>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3>Number Animation</h3>
                    <div>
                        <div class="gaper is-auto">
                            <div
                                class="link is-shade"
                                @click="v_anim_num = 12345"
                            >
                                [ 12,345 ]
                            </div>
                            <div
                                class="link is-shade"
                                @click="v_anim_num = -10000"
                            >
                                [ -10,000 ]
                            </div>
                            <div
                                class="link is-shade"
                                @click="v_anim_num = 17.25"
                            >
                                [ 17.2566 ]
                            </div>
                            <div class="filler"></div>
                            <div class="divder"></div>
                            <div class="link is-error" @click="v_anim_num = 0">
                                Reset To 0
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section">
                <p>
                    <v-number-anim
                        :value="v_anim_num"
                        :decimals="3"
                        separator=","
                        easing="linear"
                        :duration="500"
                    >
                        <template #="{ formatted, value }">
                            <div
                                class="gaper is-inline is-auto is-stacked is-center-aligned"
                            >
                                <h1>{{ formatted }}</h1>
                                <p class="meta">{{ value }}</p>
                            </div>
                        </template>
                    </v-number-anim>
                </p>
            </div>
        </div>
    </section>

    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3>Jalaali Input</h3>
                    <div>
                        <div class="gaper is-auto">
                            <div
                                class="link is-shade"
                                @click="v_date = '2022-03-20T00:00:00+03:30'"
                            >
                                [ 1400-12-29 ]
                            </div>
                            <div
                                class="link is-shade"
                                @click="v_date = '2021-03-21T00:00:00+03:30'"
                            >
                                [ 1400-01-01 ]
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section">
                <div class="meta">Minimum is 1400-01-03</div>
                <div class="input">
                    <v-jalaali-input
                        separator="/"
                        min="1400-01-03"
                        v-model="v_date"
                    />
                </div>
                <br>
                <div>
                    <div class="gaper is-auto">
                        <div class="meta">Value</div>
                        <strong class="meta is-primary">{{ v_date }}</strong>
                        <div class="divider"></div>
                        <div class="meta">Jalaali</div>
                        <strong class="meta is-primary">{{
                            jalaali(v_date)
                        }}</strong>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script lang="ts" setup>
import { vMaskInput, vNumericInput, vJalaaliInput, vNumberAnim } from "@/vComs";
import { ref } from "vue";
import { parse } from "@bardoui/date-utils";
const v_mask = ref("1234");
const v_num = ref(1234);
const v_anim_num = ref(0);
const v_formatted = ref("");
const v_date = ref("2022-01-20T00:00:00+03:30");
const jalaali = (v: string) => parse(v).format("jYYYY-jMM-jDD");
</script>
