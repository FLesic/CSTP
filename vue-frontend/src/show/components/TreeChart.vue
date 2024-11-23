<script setup lang="ts">
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { TreeChart } from 'echarts/charts';
import { TooltipComponent, ToolboxComponent } from 'echarts/components';
import VChart from 'vue-echarts';
import { onMounted, ref } from 'vue';
use([CanvasRenderer, TreeChart, TooltipComponent, ToolboxComponent]);
import * as echarts from 'echarts';
import {InfoFilled} from "@element-plus/icons-vue";
const chartRef = ref(null);
const teqTermDialogVisible = ref(false);
const companyDialogVisible = ref(false);
const patentDialogVisible = ref(false);
const rateValue = ref(0.0);
const userInfo = ref({
  user_id: 2,
  user_name: "任飞扬",
  email: "876320233@qq.com",
  phone: "18962391106",
  password: "123456"
})
const teqTermDetail = ref({
  teq_id: 0,
  teq_name: "",
  teq_field: "深度学习，人工智能，智能制造",
  teq_abstract: "深度学习是机器学习的一个子领域，它基于人工神经网络的学习方法，特别是深层神经网络。通过模仿人脑处理信息的方式，深度学习模型能够从大量数据中自动学习复杂的模式和特征，广泛应用于图像识别、语音识别、自然语言处理等领域。",
})
const companyDetail = ref({
  company_id: 0,
  company_name: "",
  company_description: "杭州炎魂网络科技有限公司，成立于2013年，专注于原创精品手机游戏研发与发行",
  contact_phone: "18962391106",
  contact_email: "876320233@qq.com",
  contact_website:"www.baidu.com",
  address:"浙江大学玉泉校区32舍",
})
const patentDetail = ref({
  patent_id: 0,
  patent_name: "",
  patent_company: "",
  patent_description: "AI技术与水墨画的结合，不仅能够学习和模仿传统水墨画的基本元素和技巧，还能够加入新的创意和表现手法，打破传统艺术的界限。AI的计算能力使得艺术家能够创造出一些传统水墨画无法表达的场景和概念，展现出超越现实的艺术想象。同时，AI还能够捕捉静态的美，并展现动态的韵律与节奏，为水墨画赋予新的生命与活力。",
  patent_keywords:"深度学习，实时水墨渲染，动态增强",
})
// TODO需要调用页面传递相关的技术名词id
let treeData = [{
  name: 'Root',
  // 技术名词层
  children: [
    {
      name: '深度学习',
      teqTerm_id: '9',
      // 企业层
      children: [
        {
          name: '杭州炎魂网络',
          company_id: '1',
          // 专利层
          children: [
            {
              name: 'AI+水墨画风',
              patent_id: '1',
            }
          ]
        },
        {
          name: '杭州智星企业',
          company_id: '2',
          children: [
            {
              name: '循环群体检索网络',
              patent_id: '2',
            },
            {
              name: '边缘监控',
              patent_id: '3',
            }
          ]
        }
      ]
    },
    {
      name: '物联网',
      teqTerm_id: '2',
      children: [
        {
          name: '杭州涂鸦科技',
          company_id: '3',
          children: [
            {
              name: '边缘网关',
              patent_id: '3',
            },
            {
              name: '循环网络下沉计算',
              patent_id: '4',
            }
          ]
        },
      ]
    },
    {
      name: '智能芯片',
      teqTerm_id: '18',
      children: [
        {
          name: '杭州智能芯片所',
          company_id: '4',
          children: [
            {
              name: '定制快造技术',
              patent_id: '5',
            },
            {
              name: '自动熔断机制',
              patent_id: '6',
            }
          ]
        },
        {
          name: '杭州创高科技园',
          company_id: '5',
          children: [
            {
              name: 'TTE安全防护',
              patent_id: '7',
            },
          ]
        }
      ]
    }
  ]
}]
onMounted(() => {
  const myChart = echarts.init(chartRef.value);
  myChart.on("dblclick", clickFun);
  const option = {
    tooltip: {
      trigger: 'item',
      triggerOn: 'mousemove'
    },
    toolbox: {
      show: true,
      feature: {
        restore: {},
        zoom: {
          show: true,
          title: {
            zoom: '放大',
            back: '还原'
          }
        }
      },
      right: 20,
      top: 20
    },
    series: [
      {
        type: 'tree',
        data: treeData,
        left: '15%',
        right: '15%',
        top: '20%',
        bottom: '20%',
        orient: 'horizontal',
        symbol: 'emptyCircle',
        symbolSize: 7,
        initialTreeDepth: 3,
        roam: true,
        label: {
          position: 'left',
          verticalAlign: 'middle',
          align: 'right',
          fontSize: 14
        },
        leaves: {
          label: {
            position: 'right',
            verticalAlign: 'middle',
            align: 'left'
          }
        },
        emphasis: {
          focus: 'descendant'
        },
        expandAndCollapse: true,
        animationDuration: 550,
        animationDurationUpdate: 750,
        layout: 'orthogonal',
        nodeGap: 20,
        zoom: 0.8
      }
    ]
  };
  myChart.setOption(option);
})
function clickFun(param) {
  if (typeof param.seriesIndex == 'undefined') {
    return;
  }
  if (param.type == 'dblclick') {
    console.log(param);
    switch (param.treeAncestors.length) {
      case 3: // 技术名词对话框
        teqTermDialogVisible.value = true;
        teqTermDetail.value.teq_id = param.data.teqTerm_id;
        teqTermDetail.value.teq_name = param.name;
        // TODO 实际中需要根据teq_id获取详情信息
        return;
      case 4: // 企业对话框
        companyDialogVisible.value = true;
        companyDetail.value.company_id = param.data.company_id;
        companyDetail.value.company_name = param.name;
        // TODO 实际中需要根据company_id获取详情信息
        return;
      case 5: // 专利对话框
          console.log(param)
        patentDialogVisible.value = true;
        patentDetail.value.patent_id = param.data.patent_id;
        patentDetail.value.patent_name = param.name;
        patentDetail.value.patent_company = param.treeAncestors[3].name;
        // TODO 实际中需要根据patent_id获取详情信息
        return;
      default:
        return;
    }

  }
}
</script>

<template>
  <el-card style="align-content: center">
      <div style="display: flex; align-items: center;">
        <div style="flex:1"></div>
        <div style="flex:4;">
          <div ref="chartRef" style="max-width: 800px; height: 400px;border: 1px solid darkgrey;"></div>
          <p style="font-size: small"><el-icon><InfoFilled /></el-icon>单击节点可进行缩放/展开,双击节点查看详情信息</p>
        </div>
        <div style="flex:1;">
          <div class="demo-rate-block">
            <span class="demonstration">匹配结果评分</span>
            <el-rate v-model="rateValue"
             size="large"
            />
          </div>
        </div>
      </div>
  </el-card>
  <!--技术名词卡片-->
  <el-dialog v-model="teqTermDialogVisible" width = "75%" align-center :show-close="false">
    <el-descriptions
        :column="2"
        size="large"
        border
    >
      <template #title>
        <span style="font-size: large">技术名词：{{teqTermDetail.teq_name}}</span>
      </template>
      <el-descriptions-item label="技术名词相关领域" span="2">{{teqTermDetail.teq_field}}</el-descriptions-item>
      <el-descriptions-item label="技术名词简介">{{teqTermDetail.teq_abstract}}</el-descriptions-item>
    </el-descriptions>
    <template #footer>
      <el-button type="primary" @click="teqTermDialogVisible=false">
        确认
      </el-button>
    </template>
  </el-dialog>

  <!--企业卡片-->
  <el-dialog v-model="companyDialogVisible" width = "75%" align-center :show-close="false">
    <el-descriptions
        direction="vertical"
        :column="3"
        size="large"
        border
    >
      <template #title>
        <span style="font-size: large">企业：{{companyDetail.company_name}}</span>
      </template>
      <el-descriptions-item label="企业联系电话">{{companyDetail.contact_phone}}</el-descriptions-item>
      <el-descriptions-item label="企业联系邮箱">{{companyDetail.contact_email}}</el-descriptions-item>
      <el-descriptions-item label="企业主页">{{companyDetail.contact_website}}</el-descriptions-item>
      <el-descriptions-item label="企业地址">{{companyDetail.address}}</el-descriptions-item>
      <el-descriptions-item label="企业简介" span="3">{{companyDetail.company_description}}</el-descriptions-item>
    </el-descriptions>
    <template #footer>
      <el-button type="primary" @click="companyDialogVisible=false">
        确认
      </el-button>
    </template>
  </el-dialog>

  <!--专利卡片-->
  <el-dialog v-model="patentDialogVisible" width = "75%" align-center :show-close="false">
    <el-descriptions
        :column="2"
        size="large"
        border
    >
      <template #title>
        <span style="font-size: large">专利：{{patentDetail.patent_name}}</span>
      </template>
      <el-descriptions-item label="所属企业">{{patentDetail.patent_company}}</el-descriptions-item>
      <el-descriptions-item label="专利关键词">{{patentDetail.patent_keywords}}</el-descriptions-item>
      <el-descriptions-item label="专利描述">{{patentDetail.patent_description}}</el-descriptions-item>
    </el-descriptions>
    <template #footer>
      <el-button type="primary" @click="patentDialogVisible=false">
        确认
      </el-button>
    </template>
  </el-dialog>
</template>

<style scoped>
.demo-rate-block {
  padding: 10px 0;
  text-align: center;
  display: inline-block;
  width: 49%;
  box-sizing: border-box;
}
.demo-rate-block .demonstration {
  display: block;
  font-size: 16px;
  margin-bottom: 5px;
}
</style>