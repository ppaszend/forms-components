<template>
  <div :class="`col col-${$props.cols}`">
    <slot />
    <div class="editor-buttons" v-if="editable">
      <button class="edit" @click="$emit('editField')">
        <font-awesome-icon icon="fa-solid fa-pen-to-square"></font-awesome-icon>
      </button>
      <button class="add" @click="$emit('addNewField')">
        <font-awesome-icon icon="fa-solid fa-plus"></font-awesome-icon>
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

defineEmits(["addNewField", "editField"]);

const $props = defineProps({
  cols: {
    type: Number,
    default: 12,
    validator: (cols: number) => cols > 0 && cols <= 12,
  },
  editable: {
    type: Boolean,
    default: false,
  },
});
</script>
<script lang="ts">
export default {
  name: "FormColumn",
};
</script>

<style lang="scss" scoped>
@use "sass:math";

.col {
  display: flex;
  position: relative;

  @for $cols from 1 through 12 {
    &.col-#{$cols} {
      width: #{(math.div(100%, 12)) * $cols};
    }
  }

  .editor-buttons {
    position: absolute;
    right: 8px;
    display: flex;
    height: 100%;
    align-items: center;
    padding: 8px 10px 0;
    gap: 6px;

    button {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 0;
      width: 30px;
      height: 30px;
      background: #42a5f5;
      color: #ffffff;
      border-radius: 15px;
      cursor: pointer;
      opacity: 0.5;
      transition: opacity 300ms ease-in-out;

      &:hover {
        opacity: 1;
      }
    }
  }
}
</style>
