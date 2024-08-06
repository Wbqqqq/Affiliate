<template>
  <div class="data-statistics">
    <div class="data-header">
      <div class="title">Data statistics</div>
      <div class="export-view" @click="exportClick">
        <img src="@/assets/export.png" class="export-icon" />
        <div class="export-text">Export</div>
      </div>
    </div>
    <div class="data-search">
      <segmented-control
        class="segmented"
        v-model="selected"
        :options="options"
      />
      <date-picker v-model="dateRang" />
    </div>
    <div class="data-table">
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column
          prop="date"
          label="Date"
          width="105"
          :resizable="false"
        >
          <template v-slot="scope">
            <div class="first-cell">
              {{ scope.row.date }}
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="visitors"
          label="Visitors"
          width="90"
          :resizable="false"
        />
        <el-table-column
          prop="views"
          label="Views"
          width="90"
          :resizable="false"
        />
        <el-table-column
          prop="trafficUserCount"
          label="Traffic User Count"
          width="90"
          :resizable="false"
        />
        <el-table-column
          prop="paymentCount"
          label="Payment Count"
          width="90"
          resizable="false"
        />
        <el-table-column
          prop="paidUserCount"
          label="Paid User Count"
          width="90"
          :resizable="false"
        />
        <el-table-column
          prop="paymentMadeByUser"
          label="Payment made by user"
          width="90"
          :resizable="false"
          show-overflow-tooltip
        >
          <template v-slot="scope">
            <div :class="getDynamicCellStyle(scope.row.paymentMadeByUser)">
              {{ scope.row.paymentMadeByUser }}
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="paymentCommission"
          label="Payment Commission"
          width="92"
          :resizable="false"
          show-overflow-tooltip
          ><template v-slot="scope">
            <div :class="getDynamicCellStyle(scope.row.paymentCommission)">
              {{ scope.row.paymentCommission }}
            </div>
          </template></el-table-column
        >
      </el-table>
    </div>
    <div class="active-content">
      <el-tabs
        v-model="activeTab"
        class="custom-tabs"
        @tab-click="handleTabClick"
      >
        <el-tab-pane label="Traffic Activity" name="tab1"
          ><ActivityList
        /></el-tab-pane>
        <el-tab-pane label="Payout Activity" name="tab2"
          ><ActivityList
        /></el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>

<script>
import { nextTick, ref, watch } from "vue";
import SegmentedControl from "@/components/SegmentedControl.vue";
import ActivityList from "@/components/ActivityList.vue";
import "element-plus/theme-chalk/el-dialog.css";
import "element-plus/theme-chalk/el-date-picker.css";
import DatePicker from "@/components/DatePicker.vue";

export default {
  components: {
    SegmentedControl,
    ActivityList,
    DatePicker,
  },

  setup() {
    // 定义选项和当前选项的引用

    const options = [
      "Today",
      "Yesterday",
      "Last 7 days",
      "Last 30 days",
      "Last 1 year",
    ];
    const selected = ref("Today"); // 初始化选中的值

    const activeTab = ref("tab1");

    const today = () => {
      const date = new Date();
      date.setHours(0, 0, 0, 0); // 将时间设置为当天的 00:00:00
      return date.toDateString();
    };

    const yesterday = () => {
      const date = new Date(today());
      date.setDate(date.getDate() - 1);
      return date.toDateString();
    };

    const lastSomeDays = (days) => {
      const endDate = new Date(today());
      const startDate = new Date(endDate);
      startDate.setDate(endDate.getDate() - days);
      return [startDate.toDateString(), endDate.toDateString()];
    };

    const formatDate = (date) => {
      const date1 = new Date(date);
      const day = String(date1.getDate()).padStart(2, "0");
      const month = String(date1.getMonth() + 1).padStart(2, "0");
      const year = date1.getFullYear();
      const a = `${day}/${month}/${year}`;
      console.log(a);
      return a;
    };

    const dateRang = ref([today(), today()]);
    const dateStr = ref(
      formatDate(dateRang.value[0]) + " - " + formatDate(dateRang.value[1])
    );

    // 监听选中的值变化
    watch(selected, (newValue, oldValue) => {
      console.log(`Selected changed from ${oldValue} to ${newValue}`);
      fetchData(newValue);
      if (newValue === "Today") {
        dateRang.value = [today(), today()];
      } else if (newValue === "Yesterday") {
        dateRang.value = [yesterday(), yesterday()];
      } else if (newValue === "Last 7 days") {
        dateRang.value = lastSomeDays(7);
      } else if (newValue === "Last 30 days") {
        dateRang.value = lastSomeDays(30);
      } else if (newValue === "Last 1 year") {
        dateRang.value = lastSomeDays(365);
      }
    });

    watch(dateRang, (newValue, oldValue) => {
      dateStr.value = formatDate(newValue[0]) + " - " + formatDate(newValue[1]);
    });

    // 模拟的网络请求函数
    const fetchData = (category) => {
      console.log(`Fetching data for ${category}...`);
      // 模拟网络请求
      setTimeout(() => {
        console.log(`Data for ${category} loaded.`);
        // 在这里处理请求到的数据
      }, 1000);
    };

    const getDynamicCellStyle = (text) => {
      const textLength = text.toString().length;
      if (textLength > 16) {
        return "small-font";
      } else if (textLength > 8) {
        return "medium-font";
      }
      return "large-font";
    };

    const exportClick = () => {
      console.log("exportClick");
    };

    const handleTabClick = (tab) => {
      console.log("当前点击的标签页:", tab);
    };

    const dateChange = (date) => {
      console.log(date);
    };

    // 返回表格数据
    const tableData = ref([
      {
        date: dateStr,
        visitors: 1234,
        views: 2123,
        trafficUserCount: 921,
        paymentCount: 126,
        paidUserCount: 98,
        paymentMadeByUser: "$999999.99",
        paymentCommission: "$999999.99",
      },
    ]);

    return {
      options,
      selected,
      tableData,
      dateRang,
      activeTab,
      exportClick,
      handleTabClick,
      getDynamicCellStyle,
      dateChange,
    };
  },
};
</script>

<style scoped>
.custom-segmented .el-segmented__item {
  margin: 0 10px;
}

.data-statistics {
  margin-top: 15px;
  padding: 15px 22px;
  background: white;
  border-radius: 8px;

  .data-header {
    display: flex;
    justify-content: space-between;
    align-items: center;

    .export-view {
      display: flex;
      align-items: center;
      cursor: pointer;
    }

    .export-icon {
      width: 12px;
      height: auto;
    }

    .export-text {
      margin-left: 5px;
      font-weight: 500;
      font-size: 10px;
      color: black;
      text-decoration-line: underline;
    }

    .export-text:hover {
      text-decoration: underline;
      text-decoration-color: var(--theme-color);
      color: var(--theme-color);
    }
  }

  .data-picker {
  }

  .data-picker:last-child {
    border-right: 0;
  }

  .data-search {
    display: flex;
    justify-content: space-between;
    align-items: center;
    justify-items: center;
    margin-top: 5px;
  }

  @media (max-width: 748px) {
    .data-search {
      display: flex;
      flex-direction: column-reverse;
      align-items: flex-start;
    }

    .data-picker {
      margin-bottom: 14px;
    }
  }
}

.data-table {
  margin-top: 17px;
}

.first-cell {
  font-size: 8px;
  font-weight: 500;
  color: #707177;
  height: 56px;
}

:deep(.el-tabs__nav-wrap::after) {
  position: static !important;
}

:deep(.el-tabs__item) {
  color: black;
  font-size: 14px;
  font-weight: 400;
}

/* 选中状态的样式 */
:deep(.el-tabs__item.is-active) {
  color: black;
  font-size: 14px;
  font-weight: bold;
}

:deep(.el-tabs__active-bar) {
  background-color: black;
}

.active-content {
  margin-top: 28px;
}

:deep .el-table--enable-row-hover .el-table__body tr:hover > td {
  background-color: rgba(0, 0, 0, 0) !important;
}

:deep(.el-tabs__header) {
  margin-bottom: 0px;
}

.large-font {
  font-size: 14px;
}

.medium-font {
  font-size: 12px;
}

.small-font {
  font-size: 10px;
}

.custom-date-picker .el-input__inner {
  border: none;
  box-shadow: none;
  outline: none;
}

.custom-date-picker .el-input {
  border: none;
}
</style>
