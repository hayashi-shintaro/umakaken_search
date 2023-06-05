<script setup lang="ts">
import {ref, inject, computed} from "vue";
import type { Area } from "@/interfaces";

interface Props {
    area: string;
}
interface Emits {
    (event: "areaSelectChanged", area:string, selectedCities:string[]): void
}

const emit = defineEmits<Emits>();
const props = defineProps<Props>();
const selectedCities:string[] = ref([]);
const areaList = inject("AreaList") as Map<string, Area>;
const targetArea = computed(
    (): Area => {
        return areaList.get(props.area) as Area;
    }
)

const onCheckBoxChanged = (): void => {
    emit("areaSelectChanged", props.area, selectedCities.value)
}
</script>

<template>
    <main>
        <h2>{{ targetArea.area }}</h2>
        <div class = "area_select">
            <template v-for="(cities, idx) in targetArea.cities">
                <label>{{ cities }}</label>
                <input type="checkbox" v-model="selectedCities" v-bind:value=cities v-on:change="onCheckBoxChanged">
            </template>
        </div>
    </main>
</template>

<style scoped>
main {
    border:red 1px solid;
}

.area_select {
    font-size: 12pt;
    width: 400px;
    display: flex;
    justify-content:flex-start;
    flex-wrap:wrap;
}
</style>