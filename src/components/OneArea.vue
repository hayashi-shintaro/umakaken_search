<script setup lang="ts">
import {ref, inject, computed} from "vue";
import type { Area } from "@/interfaces";

const selectedArea:string[] = ref([]);
const chenge_counter:numbet=ref(0);

interface Props {
    area: string;
}

interface Emits {
    (event: "areaSelectChanged", selectedArea:string[]): void
}

const emit = defineEmits<Emits>();

const props = defineProps<Props>();
const areaList = inject("AreaList") as Map<string, Area>;
const targetArea = computed(
    (): Area => {
        return areaList.get(props.area) as Area;
    }
)

const onCheckBoxChenged = (): void => {
    chenge_counter.value += 1;
    emit("areaSelectChanged", selectedArea)
}
</script>

<template>
    <main>
        <h2>{{ targetArea.area}}</h2>
        <div class = "area_select">
            <template v-for="(sub_area, idx) in targetArea.sub_area">
                <label>{{ sub_area }}</label>
                <input type="checkbox" v-model="selectedArea" v-bind:value=sub_area v-on:change="onCheckBoxChenged">
            </template>
        </div>
        <p>{{ chenge_counter }}回目の変更</p>
    </main>
</template>

<style scoped>
main {
    border:red 1px solid;
}

div {
    display: flex;
}
</style>