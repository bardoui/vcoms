<template>
    <section>
        <div class="card">
            <div></div>
            <div class="section is-attached">
                <div class="header is-left-decorated is-primary">
                    <h3 class="is-marginless">DataView</h3>
                    <div>
                        <div class="gaper is-auto">
                            <div class="link is-shade" @click="show = !show">
                                {{ show ? "Unload" : "Load" }} data
                            </div>
                            <div class="filler"></div>
                            <div class="divider"></div>
                            <div class="link" @click="error = 'Failed HTTP'">
                                Set Error
                            </div>
                            <div class="link is-error" @click="error = ''">
                                Clear Error
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="separator is-attached"></div>
            <div class="section is-secondary">
                <vDataView :data="records" :error="error">
                    <template #empty>No records exists</template>
                    <template #default="{ records }">
                        <div
                            class="gaper is-auto is-stretch-aligned is-stacked is-mobile-only"
                        >
                            <div
                                class="card"
                                v-for="(item, index) in records"
                                :key="index"
                            >
                                <div class="section">
                                    <p>
                                        <span class="is-shade-colored">{{
                                            item.id
                                        }}</span>
                                        <span>{{ ": " + item.name }}</span>
                                    </p>
                                </div>
                            </div>
                        </div>
                        <table
                            class="is-stripped is-error is-mobile-hidden is-fullwidth"
                        >
                            <thead>
                                <tr>
                                    <th>#</th>
                                    <th class="is-sortable is-sorted is-asc">
                                        Name
                                    </th>
                                    <th class="is-right-aligned">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="(item, index) in records"
                                    :key="index"
                                >
                                    <td>{{ item.id }}</td>
                                    <td class="is-sorted">{{ item.name }}</td>
                                    <td class="is-right-aligned">
                                        <span
                                            class="meta is-primary is-action"
                                            href="#"
                                            @click.prevent="action(item)"
                                            >Log</span
                                        >
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </template>
                </vDataView>
            </div>
        </div>
    </section>
</template>

<script lang="ts" setup>
import { vDataView } from "@/vComs";
import { computed, ref } from "@vue/reactivity";
const _records = ref([
    { id: 1, name: "ali" },
    { id: 2, name: "hassan" },
    { id: 3, name: "ahmad" },
    { id: 4, name: "nima" }
]);
const records = computed(() => (show.value ? _records.value : null));
const show = ref(0);
const error = ref("");
const action = (r: any) => console.log(r);
</script>
