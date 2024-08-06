<template>
  <div class="segmented-control-wrapper">
    <div class="segmented-control">
      <div
        v-for="option in options"
        :key="option"
        :class="[
          'segmented-item',
          { 'selected-item': option === selectedOption },
        ]"
        @click="selectOption(option)"
      >
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, defineComponent } from "vue";

export default defineComponent({
  name: "SegmentedControl",
  props: {
    options: {
      type: Array,
      required: true,
    },
    modelValue: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const selectedOption = ref(props.modelValue);

    const selectOption = (option) => {
      selectedOption.value = option;
      emit("update:modelValue", option);
    };

    return {
      selectedOption,
      selectOption,
    };
  },
});
</script>

<style>
.segmented-control-wrapper {
  width: 100%;
  overflow-x: hidden;
}

.segmented-control {
  display: flex;
  gap: 14px;
  overflow-x: auto;
  white-space: nowrap;
  padding: 10px 0;
}
.segmented-control::-webkit-scrollbar {
  display: none;
}

.segmented-item {
  padding: 6px 14px;
  border: 1px solid transparent;
  border-radius: 4px;
  cursor: pointer;
  transition: border-color 0.3s;
  font-size: 10px;
  font-weight: 600;
  color: black;
  background: #f7f8f8;
  flex-shrink: 0;
}

.segmented-item:hover {
  border-color: var(--theme-color);
}

.selected-item {
  border-color: var(--theme-color);
  color: var(--theme-color);
  background: white;
  border: 1px solid var(--theme-color);
}
</style>
