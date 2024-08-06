<template>
  <div class="main-content">
    <div class="container">
      <div class="module">
        <div class="balance-header">
          <div class="title">Commission Balance</div>
          <div class="balance-faq" @click="faqVisible = true">
            <img src="@/assets/help.png" class="help-icon" />
            <div>Affiliate Program FAQ</div>
          </div>
        </div>

        <div class="balance-subtitle">Commission Rate: 20%</div>
        <div class="balance-bottom">
          <div class="balance-amount">
            <div class="balance-symbol">$</div>
            <div class="balance-value">1234</div>
          </div>
          <div class="balance-withdraw" @click="withdrawClick">Withdraw</div>
        </div>
      </div>
      <div class="module">
        <div class="title">Affiliate Program Li</div>
        <div class="link-content">
          <div class="link-text">hattp：//WWW.efhafhehafhehafhehafhes…</div>
          <div class="link-copy" @click="copyClick">Copy</div>
        </div>
      </div>
    </div>
    <DataTable />
    <el-dialog
      v-model="faqVisible"
      :width="dialogWidth"
      center
      :show-close="false"
    >
      <template #header="{ close }">
        <div class="dialog-header">
          <img src="@/assets/left_horn.png" class="horn" />
          <div>Payment Terms</div>
          <img src="@/assets/right_horn.png" class="horn" />
          <img src="@/assets/close.png" class="close" @click="close" />
        </div>
      </template>
      <span class="dialog-content">
        The key payment terms of the Affiliate Program are outlined as follows:
        1. When someone signs up using your affiliate link, their account is
        assigned to your affiliate code and any future purchases they make are
        linked to you. 2. Commission is only issued based on the actual revenue
        collected by us. You receive a commission of x% of the net profit of
        each sale in USD, up to $300 per transaction. 3. As long as customers
        continue to pay, no matter Initial subscription payments/subscription
        renewals or pay as you go purchase, you receive commissions for payments
        during the first 12 months. 4. Payments are disbursed within the first
        ten working days of the following month where we received payment. 5. A
        minimum threshold of $50 is required for payment releases. All payments
        are executed via PayPal exclusively. All payments are made in USD.
      </span>
      <template #footer>
        <div class="dialog-footer">
          <el-button
            color="#var(--theme-color)"
            @click="faqVisible = false"
            class="confirm-button"
            >ok</el-button
          >
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import DataTable from "./DataTable.vue";
import { ref, onUnmounted } from "vue";

export default {
  components: {
    DataTable,
  },

  setup() {
    const faqVisible = ref(false);

    const withdrawClick = () => {
      console.log("withdrawClick");
    };

    const copyClick = () => {
      console.log("copyClick");
    };

    const dialogWidth = ref("500px");

    const updateDialogWidth = () => {
      dialogWidth.value = window.innerWidth < 748 ? "300px" : "500px";
    };

    updateDialogWidth();

    window.addEventListener("resize", updateDialogWidth);

    onUnmounted(() => {
      window.removeEventListener("resize", updateDialogWidth);
    });

    return {
      faqVisible,
      withdrawClick,
      copyClick,
      dialogWidth,
    };
  },
};
</script>

<style scoped lang="scss">
.main-content {
  max-width: 781px; // 最大宽度
  margin: 20px auto;
  padding: 0 0px;
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: stretch;
  gap: 10px;

  .module {
    flex: 1;
    background-color: white;
    padding: 10px 22px;
    box-sizing: border-box;
    text-align: center;
    border-radius: 4px;
    display: flex;
    align-items: flex-start;
    flex-direction: column;
  }
}

// 媒体查询用于在屏幕宽度小于768px时调整布局
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }

  .dialog-header {
    font-size: 16px !important;
  }

  .close {
    width: 24px !important;
    height: 24px !important;
  }
}

.balance-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;

  .balance-faq {
    font-size: 10px;
    color: #3dd598;
    font-weight: 500;
    display: flex;
    align-items: center;
    cursor: pointer;
  }

  .balance-faq:hover {
    text-decoration: underline;
    text-decoration-color: var(--theme-color);
    color: var(--theme-color);
  }

  .help-icon {
    margin-right: 3px;
    width: 10px;
    height: 10px;
  }
}

.balance-subtitle {
  margin-top: 4px;
  color: #707177;
  font-size: 10px;
}

.balance-bottom {
  height: auto;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 9px;

  .balance-amount {
    display: flex;
    align-items: center;
  }

  .balance-symbol {
    font-size: 12px;
    font-weight: 500;
    color: #000000;
    margin-right: 5px;
  }

  .balance-value {
    font-size: 24px;
    font-weight: bold;
    line-height: 32px;
    color: #000000;
  }

  .balance-withdraw {
    font-size: 12px;
    color: #ff7839;
    font-weight: bold;
    cursor: pointer;
  }

  .balance-withdraw:hover {
    text-decoration: underline;
    text-decoration-color: var(--theme-color);
    color: var(--theme-color);
  }
}

.link-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 26px;
  width: 100%;

  .link-text {
    flex: 1;
    text-align: left;
    padding: 7px 8px;
    margin-right: 18px;
    background: #f8f9fd;
    border-radius: 4px;
    font-size: 10px;
    font-weight: 400;
    color: #0062ff;
  }

  .link-copy {
    color: #0062ff;
    font-size: 12px;
    font-weight: bold;
    cursor: pointer;
  }

  .link-copy:hover {
    text-decoration: underline;
    text-decoration-color: var(--theme-color);
    color: var(--theme-color);
  }
}

.confirm-button {
  width: 100%;
  font-weight: 500;
  font-size: 16px;
  color: #ffffff;
}

.confirm-button:hover {
  background-color: #e15e1e; /* 背景颜色 */
  border-color: #e15e1e; /* 边框颜色 */
  color: #fff; /* 文字颜色 */
}

.dialog-header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  font-weight: bold;
  font-size: 20px;
  color: rgba(0, 0, 0, 0.95);
}

.horn {
  width: 21px;
  height: 22px;
  margin-left: 2px;
  margin-right: 2px;
}

.close {
  position: absolute;
  right: 18px;
  top: 10px;
  width: 32px;
  height: 32px;
}

.dialog-content {
  font-weight: 500;
  font-size: 12px;
  color: rgba(0, 0, 0, 0.95);
  text-align: left;
}
</style>
