<template>
  <div class="field-wrapper">
    <label :for="$props.for">{{ $props.label }}</label>
    <slot />
  </div>
</template>

<script lang="ts" setup>
import props from "@/helpers/props";

const $props = defineProps({
  label: props.label(),
  for: props.name(),
});
</script>
<script lang="ts">
export default {
  name: "FieldWrapper",
};
</script>

<style scoped>
.field-wrapper {
  display: flex;
  flex-direction: column;
  margin: 10px;
  position: relative;
}
</style>
