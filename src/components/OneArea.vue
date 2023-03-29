<script setup lang="ts">
import {ref, inject, computed} from "vue";
import type { Area } from "@/interfaces";

interface Props {
    area: string;
}
interface Emits {
    (event: "areaSelectChanged", area:string, selectedSubArea:string[]): void
}

const emit = defineEmits<Emits>();
const props = defineProps<Props>();
const selectedSubArea:string[] = ref([]);
const areaList = inject("AreaList") as Map<string, Area>;
const targetArea = computed(
    (): Area => {
        return areaList.get(props.area) as Area;
    }
)

const onCheckBoxChenged = (): void => {
    emit("areaSelectChanged", props.area, selectedSubArea.value)
}
</script>

<template>
    <main>
        <h2>{{ targetArea.area}}</h2>
        <div class = "area_select">
            <template v-for="(sub_area, idx) in targetArea.sub_area">
                <label>{{ sub_area }}</label>
                <input type="checkbox" v-model="selectedSubArea" v-bind:value=sub_area v-on:change="onCheckBoxChenged">
            </template>
            <p>{{ selectedSubArea }}</p>
        </div>
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