<script setup>

import {Search} from "@element-plus/icons-vue";
import {ref} from "vue";
const teqInput = ref("")
</script>

<template>
  <div style="height: 2vw"></div>
  <el-input v-model="teqInput" style="max-width: 50vw"
            :autosize="{ minRows: 12, maxRows: 12 }"
            type="textarea"
            placeholder="请输入专利/科创成果信息">
  </el-input>
  <div style="height: 2vw"></div>
  <el-button :icon="Search" round size="large">
    <a href="/show">Search</a>
  </el-button>
</template>

<style scoped>
a {
  text-decoration: none;
  color: black; /* 替换为你希望的颜色 */
}

/* 移除悬停、点击和访问过状态下的下划线 */
a:hover,
a:active,
a:visited {
  text-decoration: none;
}
</style>