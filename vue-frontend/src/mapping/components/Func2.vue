<script setup>

import {RefreshLeft, Search} from "@element-plus/icons-vue";
import { ref } from 'vue'
const checkboxGroup = ref([])
const teqTerms = ref([
  {
    teqId: '1',
    teqName: '无人机',
  },
  {
    teqId: '2',
    teqName: '物联网',
  },
  {
    teqId: '3',
    teqName: '区块链',
  },
  {
    teqId: '4',
    teqName: '数据存储',
  },
  {
    teqId: '5',
    teqName: '数据标注',
  },
  {
    teqId: '6',
    teqName: '智慧能源',
  },
  {
    teqId: '7',
    teqName: '公共安全',
  },
  {
    teqId: '8',
    teqName: '智慧交通',
  },
  {
    teqId: '9',
    teqName: '深度学习',
  },
  {
    teqId: '10',
    teqName: '指纹识别',
  },
  {
    teqId: '11',
    teqName: '智能物流',
  },
  {
    teqId: '12',
    teqName: '强化学习',
  },
  {
    teqId: '13',
    teqName: '智能手机',
  },
  {
    teqId: '14',
    teqName: '语言大模型',
  },
  {
    teqId: '15',
    teqName: '工业机器人',
  },
  {
    teqId: '16',
    teqName: '传统机器学习',
  },
  {
    teqId: '17',
    teqName: '计算机视觉',
  },
  {
    teqId: '18',
    teqName: '智能芯片',
  },
]);
</script>

<template>
  <div style="height: 2vw"></div>
  <el-checkbox-group v-model="checkboxGroup" size="large" :max="3">
    <el-row :gutter="8">
        <el-col v-for="teqTerm in teqTerms" :key="teqTerm.teqId" :span="4">
          <el-checkbox :label="teqTerm.teqName" :value="teqTerm.teqId">
            <span style="font-size:large">{{ teqTerm.teqName }}</span>
          </el-checkbox>
        </el-col>
    </el-row>
  </el-checkbox-group>
  <div style="height: 2vw"></div>
  <div>
    <el-button :icon="RefreshLeft" round size="large" @click="checkboxGroup=[]">Clean</el-button>
    <el-button :icon="Search" round size="large">Search</el-button>
  </div>
</template>

<style>
</style>