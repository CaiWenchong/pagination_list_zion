<script setup lang="ts">
import pagination from "./components/pagination.vue";
import { reactive, ref } from "vue";


const globalData = reactive({ value: "http://shipin.cdxyhpx.com/sv/3bba3a21-18926843f74/3bba3a21-18926843f74.mp4",componentName: "分页列表-zion", });
const setGlobalData = (obj: any) => {
  globalData.value = obj.value;
};


const url = ref("");
const actionflow_id = ref("5e5e155d-7b3c-4771-bf94-ceb597cbb666");

const where = ref("");
const limit = ref(-1);
const table_name = ref("");
</script>
<template>
  <pagination
    :where="where"
    :limit="limit"
    :table_name="table_name"
    :url="url"
    :actionflow_id="actionflow_id"
    :global-data="globalData"
    :set-global-data="setGlobalData"
  />

</template>

<style>
#app {
  /* height: 60vh; */
  background-color: #ffffff;
}
</style>
