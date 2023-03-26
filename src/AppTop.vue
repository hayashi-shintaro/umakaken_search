<script setup lang="ts">
import { ref, reactive, provide } from "vue";
import type { Area } from "@/interfaces";
import OneArea from "@/components/OneArea.vue"

const areaListInit = new Map<string, Area>;
areaListInit.set("福岡地区",{area:"福岡地区", sub_area:["朝倉市","春日市"]});
areaListInit.set("北九州市",{area:"北九州市", sub_area:["小倉南区", "小倉北区", "八幡東区"]} );
areaListInit.set("福岡市", {area:"福岡市", sub_area:["早良区", "中央区", "博多区"]});
provide("AreaList", reactive(areaListInit));
//const areaListInit:string[]=["博多区", "八幡東区", "行橋市"];
//const areaList = ref(areaListInit);
const emitsData:string[] = ref([])

const OnareaSelectChanged = (selectedArea:string[]):void => {
  emitsData.value = selectedArea
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
  <p>子コンポーネントからEmitsでもらったデータ{{ emitsData }}</p>
</template>

<style>
h1 {
  border:blue 1px solid;
  padding:10px;
}
</style>