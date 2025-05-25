<template>
  <div class="min-h-[1024px] bg-gray-50">
    <div class="flex h-screen">
      <div class="w-64 bg-white border-r border-gray-200">
        <div class="h-16 flex items-center px-6 border-b border-gray-200">
          <span class="text-2xl font-['Pacifico'] text-primary">logo</span>
        </div>
        <el-menu default-active="1" class="p-4">
          <el-menu-item index="1" class="flex items-center px-4 py-3 text-sm font-medium !rounded-button">
            <el-icon><DataLine /></el-icon>
            <span>数据库管理</span>
          </el-menu-item>
          <el-menu-item index="2" class="flex items-center px-4 py-3 text-sm font-medium !rounded-button mt-2">
            <el-icon><Management /></el-icon>
            <span>翻译配置</span>
          </el-menu-item>
          <el-menu-item index="3" class="flex items-center px-4 py-3 text-sm font-medium !rounded-button mt-2">
            <el-icon><Timer /></el-icon>
            <span>日志查看</span>
          </el-menu-item>
        </el-menu>
      </div>

      <div class="flex-1">
        <header class="h-16 bg-white border-b border-gray-200 px-8 flex items-center justify-between">
          <h1 class="text-lg font-medium">数据库管理</h1>
          <div class="flex items-center">
            <span class="text-sm text-gray-600 mr-4">王小明</span>
            <el-button text class="text-sm text-gray-600 hover:text-primary whitespace-nowrap !rounded-button">
              <el-icon><SwitchButton /></el-icon>
            </el-button>
          </div>
        </header>

        <main class="p-8">
          <div class="bg-white rounded-lg shadow">
            <div class="p-6 border-b border-gray-200">
              <div class="flex items-center justify-between">
                <div class="flex space-x-4">
                  <el-button type="primary" class="whitespace-nowrap !rounded-button">新增配置</el-button>
                  <el-button class="whitespace-nowrap !rounded-button">批量导入</el-button>
                  <el-button class="whitespace-nowrap !rounded-button">导出数据</el-button>
                </div>
                <el-input v-model="searchQuery" placeholder="搜索配置" class="w-64">
                  <template #prefix>
                    <el-icon><Search /></el-icon>
                  </template>
                </el-input>
              </div>
            </div>

            <el-table :data="tableData" style="width: 100%">
              <el-table-column label="源语言" prop="sourceLanguage" />
              <el-table-column label="目标语言" prop="targetLanguage" />
              <el-table-column label="翻译状态">
                <template #default="scope">
<!--                  <el-tag :type="getTagType(scope.row.status)">{{ scope.row.status }}</el-tag>-->
                </template>
              </el-table-column>
              <el-table-column label="更新时间" prop="updateTime" />
              <el-table-column label="操作" width="200">
                <template #default="scope">
                  <el-button text type="primary" class="whitespace-nowrap !rounded-button">编辑</el-button>
                  <el-button text type="danger" class="whitespace-nowrap !rounded-button">删除</el-button>
                </template>
              </el-table-column>
            </el-table>

            <div class="px-6 py-4 flex items-center justify-between border-t border-gray-200">
              <div>
                <p class="text-sm text-gray-700">
                  显示第 <span class="font-medium">1</span> 到 <span class="font-medium">10</span> 条，共 <span class="font-medium">20</span> 条记录
                </p>
              </div>
              <el-pagination
                v-model:current-page="currentPage"
                :page-size="10"
                :total="20"
                layout="prev, pager, next"
              />
            </div>
          </div>

          <div class="mt-8 bg-white rounded-lg shadow">
            <div class="p-6 border-b border-gray-200">
              <h2 class="text-lg font-medium">翻译进度统计</h2>
            </div>
<!--            <div class="p-6">-->
<!--              <div ref="chartRef" style="height: 400px;" />-->
<!--            </div>-->
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';
import { DataLine, Management, Timer, Search, SwitchButton } from '@element-plus/icons-vue';

const searchQuery = ref('');
const currentPage = ref(1);
const chartRef = ref<HTMLElement>();

const tableData = ref([
  {
    sourceLanguage: '中文',
    targetLanguage: '英文',
    status: '已完成',
    updateTime: '2024-01-15 14:30',
  },
  {
    sourceLanguage: '中文',
    targetLanguage: '日文',
    status: '进行中',
    updateTime: '2024-01-15 10:15',
  },
  {
    sourceLanguage: '中文',
    targetLanguage: '韩文',
    status: '未开始',
    updateTime: '2024-01-14 16:45',
  },
]);

const getTagType = (status: string) => {
  switch (status) {
    case '已完成':
      return 'success';
    case '进行中':
      return 'warning';
    default:
      return 'info';
  }
};

onMounted(() => {
  if (chartRef.value) {
    const chart = echarts.init(chartRef.value);
    const option = {
      animation: false,
      tooltip: {
        trigger: 'axis',
      },
      legend: {
        data: ['已完成', '进行中', '未开始'],
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true,
      },
      xAxis: {
        type: 'category',
        boundaryGap: false,
        data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
      },
      yAxis: {
        type: 'value',
      },
      series: [
        {
          name: '已完成',
          type: 'line',
          data: [120, 132, 101, 134, 90, 230, 210],
        },
        {
          name: '进行中',
          type: 'line',
          data: [220, 182, 191, 234, 290, 330, 310],
        },
        {
          name: '未开始',
          type: 'line',
          data: [150, 232, 201, 154, 190, 330, 410],
        },
      ],
    };
    chart.setOption(option);
  }
});
</script>

<style scoped>
.el-menu {
  border-right: none;
}

.el-menu-item.is-active {
  background-color: rgb(239 246 255);
  color: rgb(37 99 235);
}

.el-input :deep(.el-input__wrapper) {
  padding-left: 0;
}

.el-input :deep(.el-input__prefix) {
  padding-left: 12px;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>

