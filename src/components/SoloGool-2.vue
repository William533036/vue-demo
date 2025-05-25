
<template>
  <div class="min-h-[1024px] bg-gray-50">
    <div class="flex h-screen">
      <!-- 侧边栏 -->
      <div class="w-64 bg-white border-r border-gray-200">
        <div class="h-16 flex items-center px-6 border-b border-gray-200">
          <span class="text-2xl font-['Pacifico'] text-primary">logo</span>
        </div>
        <div class="p-4">
          <div
              v-for="(item, index) in menuItems"
              :key="index"
              class="menu-item"
              :class="{ active: item.active }"
              @click="selectMenuItem(index)"
          >
            <div class="menu-icon">
              <i :class="item.icon"></i>
            </div>
            <span>{{ item.label }}</span>
          </div>
        </div>
      </div>

      <!-- 主内容区域 -->
      <div class="flex-1">
        <header class="h-16 bg-white border-b border-gray-200 px-8 flex items-center justify-between">

        </header>

        <main class="p-8">
          <div class="bg-white rounded-lg shadow">
            <div class="p-6 border-b border-gray-200">
              <div class="flex items-center justify-between">
                <div class="flex space-x-4">
                  <button class="bg-primary text-white px-4 py-2 !rounded-button hover:bg-blue-700">
                    新增配置
                  </button>
                  <button class="bg-white text-gray-700 px-4 py-2 border border-gray-300 !rounded-button hover:bg-gray-50">
                    批量导入
                  </button>
                  <button class="bg-white text-gray-700 px-4 py-2 border border-gray-300 !rounded-button hover:bg-gray-50">
                    导出数据
                  </button>
                </div>
                <div class="relative">
                  <input
                      type="text"
                      placeholder="搜索配置"
                      class="w-64 pl-10 pr-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent"
                  />
                  <i class="fas fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
                </div>
              </div>
            </div>

            <table class="table">
              <thead>
              <tr>
                <th>源语言</th>
                <th>目标语言</th>
                <th>翻译状态</th>
                <th>更新时间</th>
                <th>操作</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(row, index) in tableData" :key="index">
                <td>{{ row.sourceLanguage }}</td>
                <td>{{ row.targetLanguage }}</td>
                <td>
                  <span :class="`tag tag-${row.status.toLowerCase()}`">{{ row.status }}</span>
                </td>
                <td>{{ row.updateTime }}</td>
                <td>
                  <button class="text-primary hover:text-blue-700 mr-4">编辑</button>
                  <button class="text-red-600 hover:text-red-700">删除</button>
                </td>
              </tr>
              </tbody>
            </table>

            <div class="px-6 py-4 flex items-center justify-between border-t border-gray-200">
              <div>
                <p class="text-sm text-gray-700">
                  显示第 <span class="font-medium">1</span> 到 <span class="font-medium">10</span> 条，共
                  <span class="font-medium">20</span> 条记录
                </p>
              </div>
              <div class="flex items-center space-x-2">
                <button
                    class="px-3 py-1 border border-gray-300 rounded-md hover:bg-gray-50 disabled:opacity-50"
                    disabled
                >
                  上一页
                </button>
                <button class="px-3 py-1 bg-primary text-white rounded-md">1</button>
                <button class="px-3 py-1 border border-gray-300 rounded-md hover:bg-gray-50">2</button>
                <button class="px-3 py-1 border border-gray-300 rounded-md hover:bg-gray-50">下一页</button>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";

export default {
  setup() {
    // 菜单项数据
    const menuItems = reactive([
      { label: "数据库管理", icon: "fas fa-database", active: true },
      { label: "翻译配置", icon: "fas fa-cog", active: false },
      { label: "日志查看", icon: "fas fa-history", active: false },
    ]);

    // 表格数据
    const tableData = reactive([
      {
        sourceLanguage: "中文",
        targetLanguage: "英文",
        status: "已完成",
        updateTime: "2024-01-15 14:30",
      },
      {
        sourceLanguage: "中文",
        targetLanguage: "日文",
        status: "进行中",
        updateTime: "2024-01-15 10:15",
      },
      {
        sourceLanguage: "中文",
        targetLanguage: "韩文",
        status: "未开始",
        updateTime: "2024-01-14 16:45",
      },
    ]);

    // 选择菜单项
    const selectMenuItem = (index) => {
      menuItems.forEach((item, i) => {
        item.active = i === index;
      });
    };

    return {
      menuItems,
      tableData,
      selectMenuItem,
    };
  },
};
</script>

<style scoped>
/* 样式部分 */
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.menu-item {
  display: flex;
  align-items: center;
  padding: 12px 16px;
  margin-top: 8px;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}
.menu-item:hover {
  background-color: rgb(239 246 255);
}
.menu-item.active {
  background-color: rgb(239 246 255);
  color: rgb(37 99 235);
}
.menu-icon {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.table {
  width: 100%;
  border-collapse: collapse;
}
.table th,
.table td {
  padding: 12px 16px;
  text-align: left;
  border-bottom: 1px solid #e5e7eb;
}
.table th {
  background-color: #f9fafb;
  font-weight: 500;
}
.tag {
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 12px;
}
.tag-success {
  background-color: #ecfdf5;
  color: #059669;
}
.tag-warning {
  background-color: #fffbeb;
  color: #d97706;
}
.tag-info {
  background-color: #f3f4f6;
  color: #4b5563;
}
</style>