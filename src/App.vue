<script setup lang="ts">
import pagination from "./components/pagination.vue";
import { reactive, ref } from "vue";

const globalData = reactive({ value: "http://shipin.cdxyhpx.com/sv/3bba3a21-18926843f74/3bba3a21-18926843f74.mp4",componentName: "分页列表-zion" });
const setGlobalData = (obj: any) => {
  globalData.value = obj.value;
};

// const url = ref("https://zion-app.functorz.com/zero/JmAxbl1kYqo/api/graphql-v2");
// const actionflow_id = ref("5e5e155d-7b3c-4771-bf94-ceb597cbb666")

// const url = ref("https://zion-app.functorz.com/zero/1qjzLgQq1g0/api/graphql-v2");
// const actionflow_id = ref("5e5e155d-7b3c-4771-bf94-ceb597cbb666")

const url = ref("https://zion-app.functorz.com/zero/1qjzLgQq1g0/api/graphql-v2");
const actionflow_id = ref("5e5e155d-7b3c-4771-bf94-ceb597cbb666")


const company_id = ref(1)
const redirectUrl = ref("https://g3n8pwgr67w62.cave.functorz.com/")
const redirectUrlParams = ref("cp_0c40c460?shangpingid=7")



</script>

<template>
  <pagination
  source="http://shipin.cdxyhpx.com/sv/3bba3a21-18926843f74/3bba3a21-18926843f74.mp4"
    vid="a175d6b01b4271ee80980764b3ec0102"
    playauth=""
    :url="url"
    :actionflow_id="actionflow_id"

    :company_id="company_id"
    :redirectUrl="redirectUrl"
    :redirectUrlParams="redirectUrlParams"
    :global-data="globalData"
    :set-global-data="setGlobalData"
    :course_log_pk="0"
  />
</template>

<style>
#app {
  height: 60vh;
}
</style>
