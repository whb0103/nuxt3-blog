<script setup lang="ts">
import { PropType } from "vue";

defineProps({
  icon: { type: String, default: "" },
  loading: Boolean,
  disabled: Boolean,
  theme: {
    type: String as PropType<"default" | "primary" | "danger">,
    default: "primary"
  },
  size: {
    type: String as PropType<"medium" | "small">,
    default: "medium"
  }
});

const emit = defineEmits(["click"]);
const hasSlot = computed(() => typeof useSlots().default === "function");
</script>

<template>
  <button
    class="common-button"
    :disabled="disabled"
    :class="[size, theme]"
    @click="loading ? null : emit('click', $event)"
  >
    <svg-icon
      v-if="icon || loading"
      :name="loading ? 'loading' : icon"
      :class="{ 'has-text': hasSlot }"
    />
    <span>
      <slot />
    </span>
  </button>
</template>

<style lang="scss">
@import "assets/style/var";

button.common-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0 10px;
  height: 32px;
  border-radius: 3px;
  border: none;
  background: $theme-color;
  transition: $common-transition;
  box-shadow: 0 0 4px rgb(0 135 255 / 52%);
  cursor: pointer;
  color: white;

  &:hover {
    background: $theme-color-darken;
  }

  &:active {
    background: $theme-color-lighten;
  }

  &.default {
    background: #fff;
    box-shadow: 0 0 4px rgb(84 70 70 / 10%);
    color: black;
    border: 1px solid #c8c8c8;

    svg {
      fill: black;
    }

    &:hover {
      background: #f6f6f6;
    }

    &:active {
      background: #f6fffe;
    }
  }

  &.danger {
    background: #ff4c4c;
    box-shadow: 0 0 4px rgb(255 0 0 / 45%);

    &:hover {
      background: #dc4040;
    }

    &:active {
      background: #ff5e5e;
    }
  }

  &:disabled {
    background: gray;
    box-shadow: 0 0 4px rgb(0 0 0 / 10%);
    cursor: not-allowed;
    color: white;
    border: none;

    &:hover {
      color: white;
      background: gray;
    }
  }

  svg {
    @include square(19px);

    fill: white;

    &.has-text {
      margin-right: 6px;
    }
  }

  span {
    font-size: 14px;
    word-break: keep-all;
  }

  &.small {
    padding: 0 6px;
    height: 25px;

    svg {
      @include square(15px);
    }

    span {
      font-size: 12px;
      line-height: 12px;
    }
  }
}
</style>
