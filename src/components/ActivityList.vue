<template>
  <div class="divider" />
  <segmented-control class="segmented" v-model="selected" :options="options" />

  <div class="activity-wrapper">
    <div
      v-if="dataStatus === 'loading'"
      class="loading-container"
      v-loading="true"
    />

    <!-- 加载失败状态 -->
    <div v-if="dataStatus === 'error'" class="loading-error">
      <div>😵 Something went wrong</div>
      <el-button type="primary" @click="loadData" class="retry-button"
        >Retry</el-button
      >
    </div>

    <!-- 空数据状态 -->
    <div v-if="dataStatus === 'empty'" class="empty">
      <img src="@/assets/empty.png" class="empty-icon" />
      <div class="empty-title">No records yet!</div>
    </div>

    <div v-if="dataStatus === 'loaded'">
      <div class="activity-list">
        <div
          v-for="payment in payments"
          :key="payment.id"
          class="activity-item"
        >
          <div class="activity-content">
            <div class="activity-title">Payment Use {{ payment.email }}</div>
            <div class="activity-row-1">
              <span>Billing Money ${{ payment.billingMoney }}</span>
              <img src="@/assets/arrow.png" class="arrow-icon" />
              <span>Commission Rate {{ payment.commissionRate }}%</span>
              <img src="@/assets/arrow.png" class="arrow-icon" />
              <span>Payment Commission ${{ payment.paymentCommission }}</span>
            </div>
            <div class="activity-row-2">
              <span>Billing ID {{ payment.billingId }}</span>
              <span>Billing Date {{ payment.billingDate }}</span>
            </div>
          </div>
        </div>
      </div>
      <NoMoreHistory />
    </div>
  </div>
</template>

<script>
import { ref, watch } from "vue";
import SegmentedControl from "@/components/SegmentedControl.vue";
import NoMoreHistory from "@/components/NoMoreHistory.vue";

export default {
  components: { NoMoreHistory, SegmentedControl },
  setup() {
    // 定义选项和当前选项的引用
    const options = ["User Register Activity", "User Payment Activity"];
    const selected = ref("User Register Activity"); // 初始化选中的值

    // 监听选中的值变化
    watch(selected, (newValue, oldValue) => {
      console.log(`Selected changed from ${oldValue} to ${newValue}`);
    });

    const dataStatus = ref("loading"); // 初始状态为加载中
    const payments = ref([]);

    // 模拟数据加载
    const loadData = () => {
      dataStatus.value = "loading"; // 切换到加载中状态
      setTimeout(() => {
        // 模拟网络请求
        const success = Math.random() > 0.3; // 70% 概率成功
        if (success) {
          payments.value = [
            {
              id: 1,
              email: "Payment Use dahfuahf****@gmail.com",
              billingMoney: "X",
              commissionRate: "a",
              paymentCommission: "Y",
              billingId: "fasfeugfe42453****sfs",
              billingDate: "17:25:56 4/23/2024",
            },
            {
              id: 2,
              email: "Payment Use dahfuahf****@gmail.com",
              billingMoney: "X",
              commissionRate: "a",
              paymentCommission: "Y",
              billingId: "fasfeugfe42453****sfs",
              billingDate: "17:25:56 4/23/2024",
            },
            {
              id: 3,
              email: "Payment Use dahfuahf****@gmail.com",
              billingMoney: "X",
              commissionRate: "a",
              paymentCommission: "Y",
              billingId: "fasfeugfe42453****sfs",
              billingDate: "17:25:56 4/23/2024",
            },
          ];
          dataStatus.value = payments.value.length ? "loaded" : "empty";
        } else {
          dataStatus.value = "error";
        }
      }, 1000); // 模拟1秒的加载时间
    };

    loadData();

    return {
      options,
      selected,
      payments,
      dataStatus,
      loadData,
    };
  },
};
</script>

<style scoped>
.activity-wrapper {
  min-height: 450px;
  background: white;
}

.divider {
  width: 100%;
  height: 1px;
  background: #f1f1f5;
}

.activity-list {
  display: flex;
  flex-direction: column;
}

.activity-item {
  margin-top: 15px;
  background: #f7f8f8;
  border-radius: 8px;
  padding: 13px 18px;
}

.activity-content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex-wrap: wrap;
}

.activity-row-1 {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex: 1;
  margin-top: 13px;
  font-weight: 500;
  font-size: 14px;
  color: #000000;
}

.activity-row-2 {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex: 1;
  margin-top: 13px;
  font-weight: 400;
  font-size: 12px;
  color: #707177;
}

.activity-title {
  font-size: 14px;
  color: #707177;
  font-weight: 400;
}

/* 当屏幕宽度小于600px时，将方向改为纵向 */
@media (max-width: 600px) {
  .activity-content {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }
  .activity-row-1 {
    margin-top: 10px;
    flex-direction: column;
    align-items: center;
  }

  .activity-row-2 {
    margin-top: 10px;
    flex-direction: column;
    align-items: center;
  }

  .arrow-icon {
    transform: rotate(90deg);
    transform-origin: center;
    margin-top: 3px;
    margin-bottom: 3px;
  }
}

.loading-container {
  width: 100%;
  height: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-error {
  text-align: center;
  margin-top: 51px;
  font-weight: 400;
  font-size: 14px;
  color: #000000;
}

.retry-button {
  margin-top: 20px;
  font-weight: bold;
  font-size: 12px;
  color: #ffffff;
  padding: 5px 28px;
}

.retry-button:hover {
  background-color: #e15e1e; /* 背景颜色 */
  border-color: #e15e1e; /* 边框颜色 */
  color: #fff; /* 文字颜色 */
}

.empty,
.data-item {
  text-align: center;
  margin-top: 20px;
}

.empty-icon {
  width: 149px;
}

.arrow-icon {
  width: 15px;
  height: 13px;
  transition: transform 0.3s ease;
}

.empty-title {
  margin-top: 12px;
  font-weight: 600;
  font-size: 14px;
  color: #000000;
}
</style>
