<script setup lang="ts">
import { ref, reactive, provide, computed } from "vue";
import type { Area } from "@/interfaces";
import OneArea from "@/components/OneArea.vue"
import { stringifyExpression } from "@vue/compiler-core";

const areaListInit = new Map<string, Area>();
areaListInit.set("福岡地区",{area:"福岡地区", sub_area:["朝倉市","春日市"]});
areaListInit.set("北九州市",{area:"北九州市", sub_area:["小倉南区", "小倉北区", "八幡東区"]} );
areaListInit.set("福岡市", {area:"福岡市", sub_area:["早良区", "中央区", "博多区"]});
provide("AreaList", reactive(areaListInit));

const allSelectedInfo = reactive(new Map<string, string[]>());
const allSelectedSubArea = computed(
  (): string[] => {
    let allSubArea:string[] = [];
    for (const [ key, value ] of allSelectedInfo) {
      for (const sub of value) {
        allSubArea.push(sub);
      }
    }
    return allSubArea;
  }
)

const OnareaSelectChanged = (area:string, selectedSubArea:string[]):void => {
  allSelectedInfo.set(area, selectedSubArea);
}
</script>

<template>
  <header>
    <h1>うまかけん福岡非公式検索ページ</h1>
  </header>
  <h4>エリア選択</h4>
  <div>
    <OneArea 
      v-for="area in areaListInit.keys()"
      v-bind:key="area"
      v-bind:area="area"
      v-on:areaSelectChanged="OnareaSelectChanged" />
  </div>
  <p>{{ allSelectedInfo }}</p>
  <p>{{ allSelectedSubArea }}</p>
</template>

<style>
h1 {
  border:blue 1px solid;
  padding:10px;
}
</style>