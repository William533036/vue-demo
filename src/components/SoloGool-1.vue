<template>
  <div class="min-h-[1024px] bg-gray-50">
    <div class="flex h-screen">
      <div class="w-64 bg-white border-r border-gray-200">
        <div class="h-16 flex items-center px-6 border-b border-gray-200">
          <span class="text-2xl font-['Pacifico'] text-blue-600">logo</span>
        </div>
        <nav class="p-4">
          <button
              v-for="(item, index) in menuItems"
              :key="index"
              :class="{
              'flex items-center px-4 py-3 text-sm font-medium w-full !rounded-button mt-2': true,
              'bg-blue-50 text-blue-600': activeMenu === item.id,
              'hover:bg-gray-50': activeMenu !== item.id
            }"
              @click="activeMenu = item.id"
          >
            <i :class="item.icon" class="mr-3"></i>
            <span>{{ item.label }}</span>
          </button>
        </nav>
      </div>

      <div class="flex-1">
        <header class="h-16 bg-white border-b border-gray-200 px-8 flex items-center justify-between">
          <h1 class="text-lg font-medium">数据库管理</h1>
          <div class="flex items-center">
            <span class="text-sm text-gray-600 mr-4">王小明</span>
            <button class="text-sm text-gray-600 hover:text-blue-600 !rounded-button p-2">
              <i class="fas fa-power-off"></i>
            </button>
          </div>
        </header>

        <main class="p-8">
          <div class="bg-white rounded-lg shadow">
            <div class="p-6 border-b border-gray-200">
              <div class="flex items-center justify-between">
                <div class="flex space-x-4">
                  <button class="bg-blue-600 text-white px-4 py-2 !rounded-button hover:bg-blue-700">新增配置</button>
                  <button class="border border-gray-300 px-4 py-2 !rounded-button hover:bg-gray-50">批量导入</button>
                  <button class="border border-gray-300 px-4 py-2 !rounded-button hover:bg-gray-50">导出数据</button>
                </div>
                <div class="relative w-64">
                  <i class="fas fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
                  <input
                      type="text"
                      v-model="searchQuery"
                      class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                      placeholder="搜索配置"
                  >
                </div>
              </div>
            </div>

            <table class="min-w-full">
              <thead class="bg-gray-50">
              <tr>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">源语言</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">目标语言</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">翻译状态</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">更新时间</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
              </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
              <tr v-for="(item, index) in tableData" :key="index">
                <td class="px-6 py-4 text-sm text-gray-900">{{ item.sourceLanguage }}</td>
                <td class="px-6 py-4 text-sm text-gray-900">{{ item.targetLanguage }}</td>
                <td class="px-6 py-4 text-sm">
                    <span :class="getStatusClass(item.status)" class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full">
                      {{ item.status }}
                    </span>
                </td>
                <td class="px-6 py-4 text-sm text-gray-900">{{ item.updateTime }}</td>
                <td class="px-6 py-4 text-sm">
                  <button class="text-blue-600 hover:text-blue-800 mr-4 !rounded-button">编辑</button>
                  <button class="text-red-600 hover:text-red-800 !rounded-button">删除</button>
                </td>
              </tr>
              </tbody>
            </table>

            <div class="px-6 py-4 flex items-center justify-between border-t border-gray-200">
              <div>
                <p class="text-sm text-gray-700">
                  显示第 <span class="font-medium">1</span> 到 <span class="font-medium">10</span> 条，共 <span class="font-medium">20</span> 条记录
                </p>
              </div>
              <div class="flex items-center space-x-2">
                <button
                    class="px-3 py-1 border border-gray-300 rounded-md !rounded-button"
                    :disabled="currentPage === 1"
                    @click="currentPage--"
                >
                  上一页
                </button>
                <span class="px-3 py-1 border border-gray-300 rounded-md bg-blue-50">{{ currentPage }}</span>
                <button
                    class="px-3 py-1 border border-gray-300 rounded-md !rounded-button"
                    :disabled="currentPage === totalPages"
                    @click="currentPage++"
                >
                  下一页
                </button>
              </div>
            </div>
          </div>

          <div class="mt-8 bg-white rounded-lg shadow">
            <div class="p-6 border-b border-gray-200">
              <h2 class="text-lg font-medium">翻译进度统计</h2>
            </div>
            <div class="p-6">
              <div ref="chartRef" style="height: 400px;" />
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';

const searchQuery = ref('');
const currentPage = ref(1);
const totalPages = ref(2);
const chartRef = ref<HTMLElement>();
const activeMenu = ref('1');

const menuItems = [
  { id: '1', label: '数据库管理', icon: 'fas fa-database' },
  { id: '2', label: '翻译配置', icon: 'fas fa-cog' },
  { id: '3', label: '日志查看', icon: 'fas fa-history' },
];

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

const getStatusClass = (status: string) => {
  switch (status) {
    case '已完成':
      return 'bg-green-100 text-green-800';
    case '进行中':
      return 'bg-yellow-100 text-yellow-800';
    default:
      return 'bg-gray-100 text-gray-800';
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
.nav-item {
  transition: all 0.3s;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>

