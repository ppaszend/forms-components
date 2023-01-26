<template>
  <div class="field-wrapper">
    <button class="drag-zone" v-if="formStore.editable">
      <font-awesome-icon icon="fa-grip-vertical"></font-awesome-icon>
    </button>
    <div class="field-wrapper-content">
      <label
        :for="$props.for"
        :contenteditable="formStore.editable"
        @click="
          (event) => {
            if (formStore.editable) {
              event.preventDefault();
            }
          }
        "
        :style="{ cursor: formStore.editable ? 'text' : 'default' }"
      >
        {{ $props.label }}
      </label>
      <slot />
    </div>
  </div>
</template>

<script lang="ts" setup>
import props from "@/helpers/shared/props";
import { useFormStore } from "@/stores/form";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

const formStore = useFormStore();

defineProps({
  label: props.label(),
  for: props.name(),
});
</script>
<script lang="ts">
export default {
  name: "FieldWrapper",
};
</script>

<style lang="scss" scoped>
.field-wrapper {
  display: flex;
  margin: 10px;
  width: 100%;
  position: relative;

  .field-wrapper-content {
    display: flex;
    flex-direction: column;
    width: 100%;
  }

  .drag-zone {
    background: #eee;
    border: 0;
    height: 100%;
    margin-right: 8px;
    border-radius: 4px;
    width: 24px;
  }
}
</style>
