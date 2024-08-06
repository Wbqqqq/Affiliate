<template>
  <div class="date-picker-wrap">
    <img src="@/assets/time-pre.png" class="time-pre" />
    <el-date-picker
      :editable="false"
      size="small"
      v-model="startDate"
      type="date"
      start-placeholder=""
      end-placeholder=""
      :clearable="false"
      @change="startChange"
    />
    <div class="divider" />
    <el-date-picker
      :editable="false"
      size="small"
      v-model="endDate"
      type="date"
      start-placeholder=""
      end-placeholder=""
      :clearable="false"
      @change="endChange"
    />
    <img src="@/assets/time-expand.png" class="time-expand" />
  </div>
</template>

<script setup lang="ts">
import { ref, shallowRef, defineEmits, defineProps, watch } from "vue";

const props = defineProps<{ modelValue: [any, any] }>();

const emit = defineEmits<{
  (e: "update:modelValue", payload: [any, any]): void;
}>();

const startDate = ref(props.modelValue[0]);
const endDate = ref(props.modelValue[1]);

const timeChange = () => {
  emit("update:modelValue", [startDate.value, endDate.value]);
};

watch(
  () => props.modelValue,
  (newValue) => {
    startDate.value = newValue[0];
    endDate.value = newValue[1];
  }
);

const startChange = (date: Date) => {
  console.log(`Type of someValue: ${typeof startDate.value}`);

  if (endDate?.value.toString().length !== 0) {
    if (Date.parse(startDate.value) > Date.parse(endDate.value)) {
      console.log("here");
      startDate.value = endDate.value;
      return;
    }
  }

  timeChange();
};
const endChange = (date: Date) => {
  if (startDate?.value.toString().length !== 0) {
    if (startDate.value > endDate.value) {
      endDate.value = startDate.value;
      return;
    }
  }

  timeChange();
};
</script>

<style scoped>
.date-picker-wrap {
  width: 188px;
  display: flex;
  align-items: center;
  height: 25px;
  background: #f7f8f8;
}

.time-pre {
  margin-left: 9px;
  width: 13px;
  height: auto;
  margin-right: 5px;
}

.time-expand {
  width: 10px;
  height: 7px;
  margin-left: 5px;
  margin-right: 5px;
}

.divider {
  width: 5px;
  height: 1px;
  background: black;
  margin-right: 2px;
  margin-left: 2px;
}

:deep(.el-input__prefix-inner) {
  display: none;
}

:deep(.el-input--small .el-input__wrapper) {
  padding: 1px 0px;
}

:deep(.el-input__wrapper) {
  background: transparent;
  box-shadow: 0 0 0 0px;
}

:deep(.el-input__wrapper:hover) {
  background: transparent;
  box-shadow: 0 0 0 0px;
}

:deep(.el-input__wrapper.is-focus) {
  background: transparent;
  box-shadow: 0 0 0 0px;
}

.el-date-editor.el-input .el-date-editor.el-input__wrapper {
  --el-date-editor-width: 60px;
}
</style>
