<script setup lang="ts">
import { ref, reactive, provide, computed } from "vue";
import type { Area } from "@/interfaces";
import OneArea from "@/components/OneArea.vue"
import { stringifyExpression } from "@vue/compiler-core";

const areaListInit = new Map<string, Area>();
areaListInit.set("福岡地区",{area:"福岡地区", cities:["太宰府市","朝倉市","筑前町","大野城市","春日市","志免町","宇美町","福津市","宗像市","古賀市","新宮町","筑紫野市","糸島市","那珂川市","久山町","粕屋町","須恵町","篠栗町","東峰村"]});
areaListInit.set("福岡市", {area:"福岡市", cities:["中央区","博多区","東区","西区","南区","城南区","早良区"]});
areaListInit.set("北九州地区",{area:"北九州地区", cities:["小倉北区", "小倉北区", "小倉南区","八幡東区","八幡西区","上毛町","若松区","水巻町","戸畑区","門司区","行橋市","岡垣町","中間市","豊前市","苅田町","築上町","みやこ町","遠賀町","芦屋町","吉富町"]} );
areaListInit.set("筑豊地区", {area:"筑豊地区", cities:["直方市","田川市","飯塚市","宮若市","桂川町","嘉麻市","香春町","小竹町","糸田町","福智町","川崎町","大任町","添田町","鞍手町"]});
areaListInit.set("筑後地区", {area:"筑後地区", cities:["久留米市", "八女市","柳川市","みやま市","大川市","大木町","大牟田市","筑後市","広川町","小郡市","大刀洗町","うきは市"]});
provide("AreaList", reactive(areaListInit));

const allSelectedInfo = reactive(new Map<string, string[]>());
const allSelectedCities = computed(
  (): string[] => {
    let allCities:string[] = [];
    for (const [ key, value ] of allSelectedInfo) {
      for (const sub of value) {
        allCities.push(sub);
      }
    }
    return allCities;
  }
)

const OnareaSelectChanged = (area:string, selectedCities:string[]):void => {
  allSelectedInfo.set(area, selectedCities);
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
  <p>{{ allSelectedCities }}</p>
</template>

<style>
h1 {
  border:blue 1px solid;
  padding:10px;
}
</style>