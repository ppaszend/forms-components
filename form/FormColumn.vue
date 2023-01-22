<template>
  <div :class="`col col-${$props.cols}`">
    <slot />
  </div>
</template>

<script lang="ts" setup>
const $props = defineProps({
  cols: {
    type: Number,
    default: 12,
    validator: (cols: number) => cols > 0 && cols <= 12,
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
  flex-shrink: 0;

  @for $cols from 1 through 12 {
    &.col-#{$cols} {
      width: #{(math.div(100%, 12)) * $cols};
    }
  }
}
</style>
