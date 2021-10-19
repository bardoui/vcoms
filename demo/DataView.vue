<template>
    <section>
        <h3>DataView</h3>
        <div class="gaper">
            <button @click="show = !show">
                {{ show ? "Unload" : "Load" }} data
            </button>
            <button @click="error = 'Failed HTTP'">Set Error</button>
            <button class="is-simple is-error" @click="error = ''">
                Clear Error
            </button>
        </div>
        <div>
            <br />
        </div>
        <div>
            <vDataView :data="records" :error="error">
                <template #empty>No records exists</template>
                <template #default="{ records }">
                    <div class="gaper is-fluid is-stacked is-mobile-only">
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
                        class="is-stripped is-error is-mobile-hidden is-fluid"
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
                            <tr v-for="(item, index) in records" :key="index">
                                <td>{{ item.id }}</td>
                                <td class="is-sorted">{{ item.name }}</td>
                                <td class="is-right-aligned">
                                    <a href="#" @click.prevent="action(item)"
                                        >Log</a
                                    >
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </template>
            </vDataView>
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
